## Initialization
초기화는 클래스, 열거형, 구조체를 사용하기 위해 인스턴스를 준비하는 작업이다.
이 과정은 인스턴스의 저장 프로퍼티 초기값을 설정하는 것을 포함하고 있고, 새로운 인스턴스가 사용되기 위해 설정되어야하는 부분에 대해서 수행한다.

이니셜라이저: 특정타입의 새로운 인스턴스가 생성될 때 호출되는 메서드. 반환 값이 없다.
디이니셜라이저(deinitializer): 클래스의 인스턴스가 메모리에서 해제되기 전에 custom clean up 작업을 수행하게 구현할 수 있다.

#### Setting Initial Values for Stored Properties
클래스와 구조체는 인스턴스가 생성되기 전에 모든 저장 프로퍼티에 적절한 초기값이 세팅되어있어야 한다.
이니셜라이저 또는 기본값 할당을 통해 초기값을 세팅할 수 있다.
* Note: 저장 프로퍼티에 기본값을 통해 초기값을 세팅하거나, 이니셜라이저를 사용할 때, 프로퍼티 감시자는 호출되지 않는다.

* Initializers
    - 매개변수가 없는 이니셜라이저
        ```swift
        struct Fahrenheit {
            var temperature: Double
            init() {
                temperature = 32.0
            }
        }
        var f = Fahrenheit()
        print("The default temperature is \(f.temperature)° Fahrenheit")
        // Prints "The default temperature is 32.0° Fahrenheit"
        ```
* Default Property Values
    - Note: 만약에 프로퍼티가 항상 같은 값의 초기값을 취해야 한다면, 이니셜라이저를 통한 초기값 할당보다 기본값을 통한 초기값 세팅을 사용해라. 결과는 똑같지만, 기본값 설정이 프로퍼티의 초기화를 (선언에) 더 밀접하게 연결한다. 이렇게 하는 것은 더 짧고 명확한 이니셜라이저를 만들 수 있게 하고, 기본값으로부터 타입을 추론할 수 있게 한다. 또한 기본값을 활용하면 더 쉽게 *기본 이니셜라이저(default initializer)* 와 *이니셜라이저 상속(intializer inheritance)* 을 사용할 수 있게 해준다.
    ```swift
    struct Fahrenheit {
      var temperature = 32.0
    }
    ```

#### Customizing Initialization
매개변수와 옵셔널 프로퍼티 타입을 가지고 초기화를 하거나, 초기화 하는 동안 상수프로퍼티를 할당하면 초기화과정을 커스터마이징 할 수 있다.

* Initialization Parameters
```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}
let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
// boilingPointOfWater.temperatureInCelsius is 100.0
let freezingPointOfWater = Celsius(fromKelvin: 273.15)
// freezingPointOfWater.temperatureInCelsius is 0.0
```
* Parameter Names and Argument Labels
이니셜라이저에 parameter name과 arguement label을 사용할 수 있다.
이니셜라이저는 함수나 메서드처럼 함수이름을갖고 있지 않다. 그래서 이니셜라이저의 매개변수의 이름과 타입은 이니셜라이저를 구분하는데 특별히 더 중요한 역할을 한다. 이거 때문에 *스위프트는 이니셜라이저안에서 모든 매개변수에 대해 자동으로 argument label을 제공한다.*
    ```swift
    struct Color {
        // 상수 프로퍼티지만 선언에 초기값이 없더라도 이니셜라이저를 통해 초기값을 준다.
        let red, green, blue: Double
        init(red: Double, green: Double, blue: Double) {
            self.red   = red
            self.green = green
            self.blue  = blue
        }
        init(white: Double) {
            red   = white
            green = white
            blue  = white
        }
    }
    let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
    let halfGray = Color(white: 0.5)

    // Argument label없이 사용하면 컴파일 에러
    // let veryGreen = Color(0.0, 1.0, 0.0)

    ```
* Initializer Parameters Without Arguement Labels
arguement label을 사용하고 싶지 않을 때, `(_)` underscore를 사용
    ```swift
    struct Celsius {
        var temperatureInCelsius: Double
        init(fromFahrenheit fahrenheit: Double) {
            temperatureInCelsius = (fahrenheit - 32.0) / 1.8
        }
        init(fromKelvin kelvin: Double) {
            temperatureInCelsius = kelvin - 273.15
        }
        init(_ celsius: Double) {
            temperatureInCelsius = celsius
        }
    }
    let bodyTemperature = Celsius(37.0)
    // bodyTemperature.temperatureInCelsius is 37.0
    ```    

* Optional Property Types
프로퍼티를 옵셔널로 선언하면 자동으로 nil이 할당된다.
```swift
class SurveyQuestion {
    var text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let cheeseQuestion = SurveyQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
// Prints "Do you like cheese?"
cheeseQuestion.response = "Yes, I do like cheese."
```
* Assigning Constant Properties During Initialization
초기화 과정 중에 어떤 시점에라도 상수프로퍼티에 값을 할당할 수 있다. 한번 상수프로퍼티에 값이 할당되면 값을 변경할 수 없다.
    - Note: 클래스의 인스턴스의 경우 , 상수프로퍼티는 프로퍼티가 정의된 곳에서만 값을 변경할 수 있고, 초기화 과정에서만 할 수 있다. subclass에서 값을 초기화할 수 없다.
```swift
class SurveyQuestion {
    let text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let beetsQuestion = SurveyQuestion(text: "How about beets?")
beetsQuestion.ask()
// Prints "How about beets?"
beetsQuestion.response = "I also like beets. (But not with cheese.)"

```

#### Default Initializers
스위프트는 (모든 프로퍼티에 기본값을 통한 초기값을 주고, 이니셜라이저가 하나도 정의되어 있지않은)클래스나 구조체에 *default initializer(기본 이니셜라이저)* 를 제공한다. 기본 이니셜라이저는 간단히 프로퍼티에 기본값을 설정하고 새로운 인스턴스를 생성한다.

* 예제 : name, quantity, purchase state가 캡슐화 되어 있는 클래스 정의
      ```swift
      class ShoppingListItem {
        // 모두 기본값을(초기값을) 제공한다. 옵셔널은 nil이 초기값
        var name: String?
        var quantity = 1
        var purchased = false
      }
      var item = ShoppingListItem()
      ```

* Memberwise Initializers for Structure Types
구조체는 사용자정의 이니셜라이저가 정의되어 있지 않다면, 자동으로 memberwise(멤버와이즈) 이니셜라이저를 받게된다.
기본 이니셜라이저와 다르게 구조체는 기본값을 가지고 있지 않은 프로퍼티가 있더라도 멤버와이즈 이니셜라이저를 받는다.
    - `init(width:height:)` 멤버와이즈 이니셜라이저
    ```swift
    struct Size {
      var width = 0.0, height = 0.0
    }
    let twoByTwo = Size(width:2.0, height: 2.0)
    ```

#### Initializer Delegation for Valeu Types
이니셜라이저가 인스턴스 초기화의 일부를 수행하기 위해 다른 이니셜라이저를 호출할 수 있다. -> `initializer delegation`, 이점 : 코드의 중복을 피할 수 있다.

초기화 위임(Initializer delegation)의 규칙은 *값 타입* 과 *클래스 타입* 이 서로 다르다.
값타입(구조체, 열거형)은 상속을 지원하지 않는다. 그래서 비교적 초기화 위임이 간단하다(제공하는 다른이니셜라이저에게 위임하는 것만 된다.(상속이 안되기 때문에)). 하지만 클래스는 상속을 지원한다. 그래서 클래스는 상속받은 모든 저장 프로퍼티가 초기화 과정 중 알맞은 할당되어 있어야 한다.

값 타입의 경우, 사용자 정의 이니셜라이저를 작성할 때 값은 값타입의 다른 이니셜라이저를 참조하기 위해 `self.init`을 사용한다. `self.init`은 이니셜라이저 안에서만 호출할 수 있다.

만약에 사용자 정의 이니셜라이저를 정의했다면, 더 이상 기본이니셜라이저나 멤버와이즈 이니셜라이저에 접근할 수 없다. 이런 제약사항은 복잡한 이니셜라이저에서 추가적인 필수 설정이 의도치않게 우회되는 상황을 방지합니다. -> 필수설정이 필요한데 제공되는 이니셜라이저에서 필요한데, 누군가 자동 이니셜라이저를 사용해서 필수설정이 안될 때를 방지한다.(자동 이니셜라이저는 기본이니셜라이저) -> 사용자정의 이니셜라이저를 통해 할 것을 기본 이니셜라이저가 할 경우를 방지한다.
* Note: 만약에 사용자정의 이니셜라이저를 정의할 때도 기본 이니실라이즈, 멤버와이즈 이니셜라이저를 사용하고 싶으면 extension(익스텐션)을 사용하여 그 안에서 구현해야 된다.
```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}

struct Rect {
    var origin = Point()
    var size = Size()
    init() {}
    init(origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}

let basicRect = Rect()
print(basicRect)

let originRect = Rect(origin: Point(x: 2.0, y: 2.0), size: Size(width: 5.0, height: 5.0))
print(originRect)

let centerRect = Rect(center: Point(x: 4.0, y:4.0), size: Size(width: 3.0, height: 3.0))

```

#### Setting a Default Property Value with a Closure or Function
만약에 저장 프로퍼티의 기본값이 사용자정의나 세팅이 필요할 때, 클로저나 전역함수(global function)를 사용할 수 있다. 프로퍼티가 속한 타입의 새로운 인스턴스가 초기화 될때 마다 클로저나 함수가 호출된다. 그리고 그 반환값이 프로퍼티의 기본값으로 할당된다.
이런 종류의 클로저나 함수는 일반적으로 프로퍼티와 같은 타입의 임시값을 만들고, 원하는 초기 상태를 나타내는 값을 조정한 다음, 임시값을 반환하여 프로퍼티의 기본값으로 사용한다.
```swift
class SomeClass {
    let someProperty: SomeType = {
        // create a default value for someProperty inside this closure
        // someValue must be of the same type as SomeType
        return someValue
    }()
}
```
`()`를 사용한것에 주목 : 즉시 실행하고 이 클로저의 반환 값을 프로퍼티에 할당, 만약에 괄호가 없으면 프로퍼티에 클로저 자체가 할당 된다.
* Note : 클로저를 프로퍼티 초기화를 위해 사용할 때 나머지 인스턴스는 초기화되어 있지 않다. 그러므로 다른 프로퍼티나 self프로퍼티, 인스턴스의 메서드를 사용할 수 없다.

* 8x8 체스판그리기 예제
```swift
struct Chessboard {
    let boardColors: [Bool] = {
       var temporaryBoard = [Bool]()
        var isBlack = false
        for i in 1...8 {
            for j in 1...8 {
                temporaryBoard.append(isBlack)
                isBlack = !isBlack
            }
            isBlack = !isBlack
        }
        return temporaryBoard
    }()
    func squareIsBlackAt(row: Int, column: Int) -> Bool {
        return boardColors[(row * 8) + column]
    }
}

let board = Chessboard()
print(board.squareIsBlackAt(row: 0, column: 1))
// Prints "true"
print(board.squareIsBlackAt(row: 7, column: 7))
// Prints "false"

```


#### 추가: UIViewController의 Initialization
* UIViewController는 두 가지 Initializer를 가지고 있으며, subclass에서 둘 다 구현하거나 둘 다 구현하지 않아야 한다.
```swift
override init(nibName nibNameOrNil: String?, bundle nibBundleOrNil: Bundle?) {
    super.init(nibName: nibNameOrNil, bundle: nibBundleOrNil)
// 초기화 내용 }
required init?(coder aDecoder: NSCoder) {
    super.init(coder: aDecoder)
// 초기화 내용 }
```
* override와 required키워드를 잊지 말 것
* 가능하다면 두가지 메소드는 구현하지 않는 것이 좋다.
* UIViewController의 초기화는 ViewController의 생명주기 중 하나인 아래 메서드에서 주로 작업한다.
```swift
override viewdidLoad() {
  super.viewDidLoad()
  // 초기화 내용
}
```
* 위 메소드가 호출될 때 모든 outlets은 연결되어 있지만 아직 화면에는 나타나지 않는 상태로, 초기화 작업을 하기 좋은 시점이다.
* 프로퍼티를 암시적 추출 옵셔널로 생성해 위 메서드에 초기화하는 전략을 사용하는 것이 좋다.
