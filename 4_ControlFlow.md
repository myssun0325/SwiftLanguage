#### For-In Loops
* when the dictionary is iterated tuple is returned(key, value)
```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
  print("\(animalName)s have \(legCount) legs")
}
// ants have 6 legs
// spiders have 8 legs
// cats have 4 legs
```
* interval주기 (`stride`)
```swift
let minutes = 60
let minuteInterval = 5
// 포함하진 않을 땐 stride(from:to:by:), 포함할 땐 stride(from:through:by:)
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
  print(tickMark)
}
// 5
// 10 ...55까지 5단위로 출력
```

#### While
* Repeat-while

#### Conditional Statements
* If
* Switch문 기본 form
    ```swift
     switch some value to consider {
      case value 1:
          respond to value 1
      case value 2,
           value 3:
          respond to value 2 or 3
      default:
          otherwise, do something else
      }
    ```
    - 모든 switch statement는 철저해야 한다. -> 반드시 switch case중에 하나엔 포함되어야 한다. 값이 가능한 모든 case를 써주거나(열거형 값) 아닐 경우 반드시 default를 작성해줘야한다.(default없을 시 에러)
    - *case문에는 실행가능한 코드가 꼭 위치해야 한다.*
    - 비교되는 value값엔 반드시 같은 타입이 들어가야 한다.
    - 매칭되는 모든 case를 제공하지 못할 경우 default case를 맨 마지막에 정의할 수 있다.
        ```swift
        let someCharacter: Character = "z"
        switch someCharacter {
        case "a":
            print("The first letter of the alphabet")
        case "z":
            print("The last letter of the alphabet")
        default:
            print("Some other character")
        }
        ```
* Switch - No Implicit Fallthrough
    - 스위프트에선 break문 없이 매칭된 case를 모두 실행하면 자동으로 switch구문이 종료된다
    - 아래 코드와 같이 "a" 와 "A" 모두 match할 수 없다.
        ```swift
        let anotherCharacter: Character = "a"
        switch anotherCharacter {
        case "a": // Invalid, the case has an empty body
        case "A":
            print("The letter A")
        default:
            print("Not the letter A")
        }
        // case다음엔 반드시 실행가능한 구문이 올 것!
        // This will report a compile-time error.
        ```
    - 두개의 case에 대해 match 하고 싶다면 comma 사용 (case를 명시적으로 연속실행하려면 `fallthrough`사용)
        ```swift
        let anotherCharacter: Character = "a"
        switch anotherCharacter {
        case "a", "A":
            print("The letter A")
        default:
            print("Not the letter A")
        }
        // Prints "The letter A"
        ```
    - Interval Matching : case 에 범위가 들어갈 수 있음 (case 0..<5:)

#### Switch - Tuples
* multiple values 테스트를 위해서 같은 switch문 안에서 tuple을 활용할 수 있음
    ```swift
    let somePoint = (1, 1)
    switch somePoint {
    case (0, 0):
        print("\(somePoint) is at the origin")
    case (_, 0):
        print("\(somePoint) is on the x-axis")
    case (0, _):
        print("\(somePoint) is on the y-axis")
    case (-2...2, -2...2):
        print("\(somePoint) is inside the box")
    default:
        print("\(somePoint) is outside of the box")
    }
    ```
#### Switch - Value Bindings
* case에 임시 상수나 변수를 사용
    ```swift
    let anotherPoint = (2, 0)
    switch anotherPoint {
      // place holder constants x, y활용하기
    case (let x, 0):
        print("on the x-axis with an x value of \(x)")
    case (0, let y):
        print("on the y-axis with a y value of \(y)")
    case let (x, y):
        print("somewhere else at (\(x), \(y))")
    }
    // 마지막 case구문으로 인해 default를 정의해주지않아도 된다.
    ```
* Where
    ```swift
    let yetAnotherPoint = (1, -1)
    switch yetAnotherPoint {
    case let (x, y) where x == y:
        print("(\(x), \(y)) is on the line x == y")
    case let (x, y) where x == -y:
        print("(\(x), \(y)) is on the line x == -y")
    case let (x, y):
        print("(\(x), \(y)) is just some arbitrary point")
    }
    ```
* Compound Cases
    - 하나라도 해당하면 case에 매치되는걸로 간주한다.
        ```swift
        let someCharacter: Character = "e"
        switch someCharacter {
        case "a", "e", "i", "o", "u":
            print("\(someCharacter) is a vowel")
        case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
             "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
            print("\(someCharacter) is a consonant")
        default:
            print("\(someCharacter) is not a vowel or a consonant")
        }

        ```
    - compound case의 value bindings
        ```swift
        let stillAnotherPoint = (9, 0)
        switch stillAnotherPoint {
        case (let distance, 0), (0, let distance):
            print("On an axis, \(distance) from the origin")
        default:
            print("Not on an axis")
        }
        // stillAnotherPoint가 0이 포함되어있지않은(예를들어 9, 3)이면 매치되는 케이스가 없으므로 default
        // Prints "On an axis, 9 from the origin"
        ```

#### Control Transfer Statements
* 5개 : `continue`, `break`, `fallthrough`, `return`, `throw`
* Continue
* Fallthrough
    - C에서 break를 안쓰는것과 같은 효과
    - fallthrough 도 break처럼 만나면 다음코드를 실행하지 않고 바로 다음 case나 default block 으로 이동

* Labeled Statements
    - 반복문이나 조건문을 중첩으로 사용 할 때, `statement label`을 사용해서 어떤 반복문이나 조건문을 break, continue할지 명시하는 것.(조건문이나 반복문에다가 이름을 붙여주는것)

#### Early Exit
* `guard`
* 항상 else문을 가지고 있음(true가 아닐 경우에만)
```swift
func greet(person: [String:String]) {
    guard let name = person["name"] else {
        return
    }

    print("Hello \(name)")

    guard let location = person["location"] else {
        print("I hop the weather is nice near you")
        return
    }

    print("I hope the weather is nice in \(location)")
}

greet(person: ["name": "John"])
greet(person: ["name": "Jane", "location": "Cupertino"])
// Hello John
// I hop the weather is nice near you
// Hello Jane
// I hope the weather is nice in Cupertino
```
* `else`코드블록이 실행되면 코드블록을 종료하기 위해서 반드시 제어명령(return, break, continue, throw와 같은)을 사용해야 한다. 아니면 fatalError와 같은 함수나 return하지 않는 메서드를 호출할 수 있다.
    - 결국 else를 사용할 때 코드블록안에서 탈출하기 위해서 위에 중에 하나 써야된다는 말(안쓰면 에러)
* `guard`를 쓰면 코드의 가독성 증가(if에 비해서)
* else 블록에서 wrapping 없이 코드를 작성할 수 있게해주고, 요구 사항 다음에 위반된 요구사항을 처리하는 코드를 유지할 수 있다.

#### Checking API Availability
* Swift에는 API가 사용가능한지 체크하는 기능이 built-in 되어있다.
```swift
if #available(iOS 10, macOS 10.12, *) {
    // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
} else {
    // Fall back to earlier iOS and macOS APIs
}
```
* general form
```swift
 if #available(platform name version, ..., *) {
    statements to execute if the APIs are available
} else {
    fallback statements to execute if the APIs are unavailable
```
