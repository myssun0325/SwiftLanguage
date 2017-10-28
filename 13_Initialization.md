## Initialization
초기화는 클래스, 열거형, 구조체를 사용하기 위해 인스턴스를 준비하는 작업이다.
이 과정은 인스턴스의 저장 프로퍼티 초기값을 설정하는 것을 포함하고 있고, 새로운 인스턴스가 사용되기 위해 설정되어야하는 부분에 대해서 수행한다.
*초기화는 인스턴스가 생성되기 전에 실행되는 부분이고, 인스턴스가 생성되기 전에 셋팅이 필요한 부분들을 수행한다.*

이니셜라이저: 특정타입의 새로운 인스턴스가 생성될 때 호출되는 메서드. 반환 값이 없다.
디이니셜라이저(deinitializer): 클래스의 인스턴스가 메모리에서 해제되기 전에 custom clean up 작업을 수행하게 구현할 수 있다.

#### Setting Initial Values for Stored Properties
클래스와 구조체는 인스턴스가 생성되기 전에 모든 저장 프로퍼티에 적절한 초기값이 세팅되어있어야 한다.(저장 프로퍼티는 불확실한(값이 없는)상태로 남아있을 수 없다.)
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
    - Note: 클래스의 인스턴스의 경우 , 상수프로퍼티는 프로퍼티가 정의된 곳에서만 값을 변경할 수 있고, 초기화 과정에서만 할 수 있다. subclass에서 값을 변경할 수 없다.
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
값타입(구조체, 열거형)은 상속을 지원하지 않는다. 그래서 비교적 초기화 위임이 간단하다(제공하는 다른이니셜라이저에게 위임하는 것만 된다.(상속이 안되기 때문에)). 하지만 클래스는 상속을 지원한다. 그래서 클래스는 상속받은 모든 저장 프로퍼티가 초기화 과정 중 적절한 값이 ㅎㅑ할당되어 있어야 한다.

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

#### Class Inheritance and Initialization
클래스의 모든 저장프로퍼티는(상속받은 프로퍼티를 포함하여) 초기화 중에 반드시 초기값이 할당되어야 한다.
클래스에는 모든 저장프로퍼티가 초기값을 받도록 해주는 두가지 종류의 이니셜라이저가 있다. *designated initializer(지정 이니셜라이저)* , *convenience initializaer(편의 이니셜라이저)*
* Designated Initializers and Convenience Initializers
    * 지정이니셜라이저는 클래스의 기본 이니셜라이저다. 지정이니셜라이저는 클래스의 모든 프로퍼티를 초기화하고 부모클래스의 초기화를 진행하기 위해 적절한 부모클래스의 이니셜라이저를 호출한다. 클래스는 일반적으로 많지 않은 편의 이니셜라이저를 가진다(보통 한개가 일반적). 편의 이니셜라이저는 초기화가 일어나는 "funnel(깔때기)" 지점이고, 편의 이니셜라이저의 초기화는 슈퍼클래스 chain에서 계속 진행된다.
    * 모든 클래스는 적어도 한개 이상의 지정이니셜라이저를 가지고 있다. (어떤 경우에 이 요구조건은 부모클래스로부터 상속받은 하나 이상의 이니셜라이저로 충족된다.)
    * *You can define a convenience initializer to call a designated initializer from the same class as the convenience initializer with some of the designated initializer’s parameters set to default values. (해석불가..)*
    : 지정 이니셜라이저의 매개변수 일부를 기본값으로 설정하여 같은 클래스 안에서 지정이니셜라이저를 호출하기 위해 편의 이니셜라이저를 정의할 수 있다. -> 지정 이니셜라이저에서 기본값을 설정하는 작업의 일부를 편의이니셜라이저를 통해 할 수 있다는 얘기..?

* Syntax for Designated and COnvenience Initializers
```swift
// 지정 이니셜라이저
init(parameters) {
  statements
}
// 편의 이니셜라이저
convenience init(parameters) {
  statements
}
```
* Initializer Delegation for Class Type.
    * 규칙1 : 지정이니셜라이저는 반드시 바로 상위 부모클래스의 지정이니셜라이저를 호출해야 한다.
    * 규칙2 : 편의 이니셜라이저는 반드시 같은 클래스 안에 다른 이니셜라이저를 호출해야 한다.
    * 규칙3 : 편의 이니셜라이저는 궁극적으로 지정 이니셜라이저를 호출해야한다.
    * 간단하게 외우기 : __지정 이니셜라이저는 반드시 위로 위임하기__, __편의 이니셜라이저는 옆(across)으로 위임하기__
    * 이니셜라이저 규칙 모식도 그림 참조하기
    * Note: 위 규칙들은 사용자가 클래스의 인스턴스를 어떻게 생성하는지에 영향을 미치지 않는다. 위 규칙에서 이니셜라이저는(편의든 지정이든) 완전히 초기화된 인스턴스를 생성하는데 사용된다. 위 규칙은 단지 어떻게 클래스의 이니셔라이저를 구현할 것인가에만 영향을 미친다.
[이니셜라이저](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-ID203)

* Two-Phase Initialization
스위프트에서 클래스 초기화는 두 단계 과정을 거친다. 첫번째 단계에는 각 저장 프로퍼티가 초기값을 할당받는다. 모든 프로퍼티의 초기상태가 결정되면 2단계를 시작한다. 2단계에서는 각 클래스는 새로운 인스턴스가 사용할 준비가 된것으로 간주되기 전에 저장프로퍼티가 커스터마이즈될 수 있는 기회를 얻는다.  
2단계-초기화를 사용하면 각 클래스가 클래스 계층에서 완전한 유연성을 제공하면서도 초기화를 안전하게 할 수 있도록 도와준다. 2단계-초기화는 프로퍼티의 값들이 초기화가 되기전에 접근되는 것을 방지하고 다른 이니셜라이저로부터 값이 변경되는 것을 막을 수 있다.

* 스위프트의 컴파일러는 2단계-초기화를 오류없이 끝내기 위해 4가지 safety-check를 한다.
    1. 지정 이니셜라이저는 반드시 부모클래스의 이니셜라이저를 위임(호출)하기 전에 자신의 클래스의 모든 프로퍼티가 초기값이 초기화되었는지 확인한다.
    2. 지정 이니셜라이저는 상속받은 프로퍼티에 값을 할당하기 전에 반드시 부모클래스의 이니셜라이저에게 위임(호출)해야한다. (상속받은 할당하기 전에 부모이니셜라이저 먼저)
    3. 편의 이니셜라이저는 다른 프로퍼티에 값을 할당하기 전에(같은 클래스에 정의된 프로퍼티도 포함해서) 반드시 다른 이니셜라이저에게 위임해야한다(호출해야한다.) (만약에 이렇게 안된다면 편의이니셜라어지가 할당한 새로운 값은 같은 클래스의 지정 이니셜라이저에 의해 덮어쓰기 된다.)
    4. 이니셜라이저는 1단계 초기화 단계가 완전히 완료되기전까지는 어떤 인스턴스 메서드도 호출 할 수 없고, 인스턴스 프로퍼티의 값도 읽을 수 없고, self(자신의 인스턴스를 가리키는)를 참조할 수도 없다.
    * 클래스의 인스턴스는 1단계초기화가 끝나기전까지 유효하지 않다. 프로퍼티는 단순히 값을 읽을 수만 있고, 메서드는 호출만 가능하다.

* 이 4가지를 근거하여 2단계 초기화가 이루어지는 과정
    1. 1단계 초기화
        1. 클래스에서 지정 또는 편의 이니셜라이저를 호출한다.
        2. 클래스의 새로운 인스턴스를 위해 메모리가 할당된다. 메모리는 아직 초기화 되지 않은 상태이다.
        3. 클래스의 지정 이니셜라이저가 클래스에 정의된 모든 저장 프로퍼티가 값을 가지고 있는지 확인한다. 이 때 이 저장프로퍼티들에 대한 메모리가 초기화 된다.
        4. 이 과정을 상속체인에 따라 상속체인 맨 위까지 클래스에 적용한다.
        5. 상속체인 맨위 클래스(최상위 클래스)까지 도착하면, 최상위 클래스는 모든 저장프로퍼티가 값을 가진 것을 보장하고, 이 때 인스턴스의 메모리가 모두 초기화 된다. 1단계 초기화 완료
    2. 2단계 초기화
        1. 역방향으로 상속체인의 맨 위(최상위 클래스)부터 아래로 내려오면서 각 지정 이니셜라이저는 인스턴스를 커스터마이즈할 수 있는 기회를 갖는다. 이 때 이니셜라이저는 `self`를 통해 값에 접근하고 값을 수정할 수 있고, 인스턴스메서드를 호출하는 등의 작업을 수행한다.
        2. 마지막으로 편의이니셜라이저가 인스턴스를 커스터마이즈할 수 있는 기회와 `self`를 사용할 수 있는 기회를 얻는다.

#### Initializer Inheritance and Overriding
스위프트의 자식클래스는 default로 부모클래스의 이니셜라이저를 상속받지 않는다. 이는 부모클래스로부터 상속받은 이니셜라이저는 자식클래스에 최적화되어 있지 않으며, 그로 인해 자식 클래스의 인스턴스가 완전하고 정확하게 초기화되지 않은 상황을 방지하고자 함이다. 만약 부모클래스에서 상속받은 이니셜라이저와 똑같은 이니셜라이저를 자식클래스에서 사용하고 싶으면 자식클래스에서 부모의 이니셜라이저를 커스텀화해서 구현해주면 된다.

부모클래스와 동일한 지정 이니셜라이저를 사용하고 싶으면 자식클래스에서 재정의(override사용)하면 된다. 기본 이니셜라이저와 편의이니셜라이를 재정의하는 경우에도 override를 사용하면된다. 자식 클래스의 편의이니셜라이저가 부모클래스의 지정 이니셜라이저를 재정의하는 경우에도 override를 사용하면 된다.(자식 클래스에서 부모클래스의 편의 이니셜라이저는 호출할 수 없기 때문에 재정의가 필요없다. = 부모클래스의 편의 이니셜라이저와 동일한 이니셜라이저를 자식클래스에 구현할 때 override를 부이지 않는다.)

* 예제
```swift
class Vehicle {
    var numberOfWheels = 0
    var description: String {
        return "\(numberOfWheels) wheel(s)"
    }
}
// 기본 이니셜라이저 사용
let vehicle = Vehicle()
print("Vehicle: \(vehicle.description)")

// a subclass of Vehicle
class Bicycle: Vehicle {
    // a custom designated initializer (superclass의 designated initializer와 동일하므로 override가 붙는다.)
    override init() {
        super.init()
        numberOfWheels = 2
    }
}
let bicycle = Bicycle()
print("Bicycle: \(bicycle.description)")
// Bicycle: 2 wheel(s)
```
* 상속받은 Bicycle이 super.init()을 호출한 것은 상속받은 프로퍼티인 `numberOfWheels`가 Bicycle이 프로퍼티를 변경하기전에 Vehicle에 의해서 초기화 됐다는 것을 보장한다. super.init()호출 이후에 numberOfWheels이 새로운 2로 값이 변경되었다.
* Note: 자식클래스에서 이니셜라이저안에서 상속받은 프로퍼티를 변경할 수 있다. 하지만 상속받은 `상수 프로퍼티`는 변경할 수 없다.

* Automatic Initializer Inheritance 이니셜라이저 자동상속
    * 위에서 언급했듯이, 부모클래스의 이니셜라이저는 자동으로 상속되지 않는다. 하지만 특정조건이 만족될 경우 자동으로 상속하게 된다. -> override를 쓰지 않아도 된다는 것을 의미한다. 사실 대부분의 경우 자식클래스에서 이니셜라이저를 재정의해줄 필요가 없다.
    * 자식클래스에서 자식클래스에서 만든 새로운 프로퍼티에 대해 모두 기본값을 제공한다고 가정했을 때 아래 두 규칙을 적용된다.
        1. 자식클래스에서 지정 이니셜라이저를 구현하지 않으면, 부모클래스의 지정 이니셜라이저가 모두 상속된다.
        2. 자식클래스에서 부모클래스의 모든 지정이니셜라이저를 재정의하거나 규칙1에 따라 자식클래스가 자동으로 지정이니셜라이저를 상속받은 경우, 다시 말해 부모의 지정 이니셜라이저를 모두 사용할 수 있는 경우 -> 부모클래스의 편의 이니셜라이저를 모두 상속받는다.
        * 이 규칙은 자식클래스가 편의클래스를 더 추가한 경우에도 적용된다.
    * Note: 자식클래스에서 부모클래스의 지정이니셜라이저를 자식클래스의 편의이니셜라이저로 정의할 수 있다. (2번째 규칙에 의해서)

* Designated and Convenience Initializers in Action
아래 코드를 해석할 줄 알면 자동상속에 대해서 이해완료
```swift
class Food {
    var name: String
    init(name: String) {
        self.name = name
    }
    convenience init() {
        self.init(name: "[Unnamed]")
    }
}

class RecipeIngredient: Food {
    var quantity: Int
    init(name: String, quantity: Int) {
        self.quantity = quantity
        super.init(name: name)
    }
    override convenience init(name: String) {
        self.init(name: name, quantity: 1)
    }
}

let namedMeat = Food(name: "Bacon")
let mysteryMeat = Food()

let oneMysterItem = RecipeIngredient()
let oneBacon = RecipeIngredient(name: "Bacon")
let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)

class ShoppingListItem: RecipeIngredient {
    var purchased = false
    var desciprtion: String {
        var output = "\(quantity) x \(name)"
        output += purchased ? " ✔" : " ✘"
        return output
    }
}

var breakfastList = [
    ShoppingListItem(),
    ShoppingListItem(name: "Bacon"),
    ShoppingListItem(name: "Eggs", quantity: 6)
]
breakfastList[0].name = "Oragne juice"
breakfastList[0].purchased = true
for item in breakfastList {
    print(item.desciprtion)
}

// 1 x Orange juice ✔
// 1 x Bacon ✘
// 6 x Eggs ✘

```

#### Failable Initializers
* `init?`을 통해 실패가능한 이니셜라이저를 정의할 수 있다.
* 실패가능한 이니셜라이저 안에 `return nil` -> 실제는 이니셜라이저는 어떤 값도 반환하지 않는다. 오히려 이것은 초기화가 끝날 때 `self`가 완전히 초기화 된 것을 보장해준다. 비록 초기화 실패를 위해서 `return nil`이 이니셜라이저가 실패했을 때 사용하지만 `reuturn`을 성공했을 때 쓰진 않는다.
```swift
let wholeNumber: Double = 12345.0
let pi = 3.14159

if let valueMaintained = Int(exactly: wholeNumber) {
    print("\(wholeNumber) conversion to Int maintains value of \(valueMaintained)")
}
// Prints "12345.0 conversion to Int maintains value of 12345"

let valueChanged = Int(exactly: pi)
// valueChanged is of type Int?, not Int

if valueChanged == nil {
    print("\(pi) conversion to Int does not maintain value")
}
// Prints "3.14159 conversion to Int does not maintain value"
```
```swift
struct Animal {
    let species: String
    init?(species: String) {
        if species.isEmpty { return nil }
        self.species = species
    }
}

let someCreature = Animal(species: "Giraffe")
// someCreature is of type Animal?, not Animal

if let giraffe = someCreature {
    print("An animal was initialized with a species of \(giraffe.species)")
}
// Prints "An animal was initialized with a species of Giraffe"

let anonymousCreature = Animal(species: "")
// anonymousCreature is of type Animal?, not Animal

if anonymousCreature == nil {
    print("The anonymous creature could not be initialized")
}
// Prints "The anonymous creature could not be initialized"

```

* Failable Initializers for Enumerations
```swift
enum TemperatureUnit {
    case kelvin, celsius, fahrenheit
    init?(symbol: Character) {
        switch symbol {
        case "K":
            self = .kelvin
        case "C":
            self = .celsius
        case "F":
            self = .fahrenheit
        default:
            return nil
        }
    }
}

let fahrenheitUnit = TemperatureUnit(symbol: "F")
if fahrenheitUnit != nil {
    print("This is a defined temperature unit, so initialization succeeded.")
}
// Prints "This is a defined temperature unit, so initialization succeeded."

let unknownUnit = TemperatureUnit(symbol: "X")
if unknownUnit == nil {
    print("This is not a defined temperature unit, so initialization failed.")
}
// Prints "This is not a defined temperature unit, so initialization failed."

```

* Failable Initializers for Enumerations with Raw Values
rawValue를 갖고있는 열거형은 자동으로 실패가능한 이니셜라이저를 받는다.`init?(rawValue:)`
```swift
enum TemperatureUnit: Character {
    case kelvin = "K", celsius = "C", fahrenheit = "F"
}

let fahrenheitUnit = TemperatureUnit(rawValue: "F")
if fahrenheitUnit != nil {
    print("This is a defined temperature unit, so initialization succeeded.")
}
// Prints "This is a defined temperature unit, so initialization succeeded."

let unknownUnit = TemperatureUnit(rawValue: "X")
if unknownUnit == nil {
    print("This is not a defined temperature unit, so initialization failed.")
}
// Prints "This is not a defined temperature unit, so initialization failed."

```
* Propagation of Initialization Failure
클래스, 구조체, 열거형의 실패가능한 이니셜라이저는 같은 클래스, 구조체, 열거형 안에서 다른 실패가능한 이니셜라이저에게 delegate할 수 있다.
이와 비슷하게 자식클래스의 실패가능한 이니셜라이저는 부모클래스의 실패가능한 이니셜라이저에게 delegate할 수 있다.
이 둘중 하나케이스에서, 만약에 실패하게 만드는 이니셜라이저를 다른 이니셜라이저에게 delegate하면 전체 초기화과정이 즉시 실패하고 코드는 더 이상 실행되지 않는다.
*Note: 실패가능한 이니셜라이저는 nonfailable이니셜라이저에게도 delegate할 수 있다. 이러한 접근은 실패하지 않는 초기화 과정에다가 실패가능성이 있는 상태를 추가해야할 경우 사용한다.*
```swift
class Product {
    let name: String
    init?(name: String) {
        if name.isEmpty { return nil }
        self.name = name
    }
}

class CartItem: Product {
    let quantity: Int
    init?(name: String, quantity: Int) {
        if quantity < 1 { return nil }
        self.quantity = quantity
        super.init(name: name)
    }
}

if let twoSocks = CartItem(name: "sock", quantity: 2) {
    print("Item: \(twoSocks.name), quantity: \(twoSocks.quantity)")
}

// 자식클래스의 이니셜라이저 실패
if let zeroShirts = CartItem(name: "shirt", quantity: 0) {
    print("Item: \(zeroShirts.name), quantity: \(zeroShirts.quantity)")
} else {
    print("Unable to initialize zero shirts")
}
// Prints "Unable to initialize zero shirts"

// 부모클래스로 delegate했는데 부모클래스의 이니셜라이저 실패
if let oneUnnamed = CartItem(name: "", quantity: 1) {
    print("Item: \(oneUnnamed.name), quantity: \(oneUnnamed.quantity)")
} else {
    print("Unable to initialize one unnamed product")
}
// Prints "Unable to initialize one unnamed product"

```

* Overriding a Failiable initializer
자식클래스에서 부모클래스의 실패가능한 이니셜라이저를 재정의할 수 있다. 실패가능한 라이저를 nonfailable 이니셜라이저로 재정의 가능. (반대경우는 불가)
만약에 이렇게 nofailable로 재정의하려면 방법은 *delegate up to the superclass innitializer to force-unwrap the result of the failable superclass initializer.* -> 슈퍼클래스의 실패가능한 이니셜라이저를 강제로 언랩핑하기
```swift
class Document {
    var name: String?
    // this initializer creates a document with a nil name value
    init() {}
    // this initializer creates a document with a nonempty name value
    init?(name: String) {
        if name.isEmpty { return nil }
        self.name = name
    }
}

class AutomaticallyNamedDocument: Document {
    override init() {
        super.init()
        self.name = "[Untitled]"
    }
    override init(name: String) {
        super.init()
        if name.isEmpty {
            self.name = "[Untitled]"
        } else {
            self.name = name
        }
    }
}

// 강제언랩핑
class UntitledDocument: Document {
    override init() {
        super.init(name: "[Untitled]")!
    }
}

```

* The `init!` Failable Intializer
`init?`대신 `init!`사용할 수 있다. `init?`을 `init!`에 delegate할 수 있다. `init?`을 `init!`으로 재정의할 수 있다. 반대도 가능
하지만 만약 이니셜라이저가 실패하면 `init!`은 assertion을 trigger한다.

#### Required Initializers
이니셜라이저 전에 `required`를 사용하면 모든 자식클래스에서 해당 이니셜라이저를 반드시 구현해야 한다는 것을 의미한다.
`required`로 구현된 이니셜라이저는 앞에 같은 키워드를 사용한다. override를 사용하면 안된다.
상속된 이니셜라이저에서 required 요구사항을 만족하면 명시적으로 required 이니셜라이저를 구현해주지 않아도 된다.
```swift
class SomeClass {
  required init() {
     // initializer implementation goes here
  }
}

class SomeSubClass: SomeClass {
  required init() {
      // subclass implementation of the required initializer goes here
  }
}
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
