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
    - else는 선택사항이다. if와 else if만 있어도 된다.

* Switch
    - 기본 형식
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
    - 모든 switch statement는 철저해야 한다. -> 비교되는 값이 반드시 switch case 중 하나에 포함되어야 한다. 값이 가능한 모든 case를 써주거나(열거형 값) 아닐 경우 반드시 default를 작성해줘야한다.(default없을 시 에러)
    - No Implicit Fallthrough
        - 스위프트의 switch문은 다음 기본적으로 case다음 case로 내려가지 않는다. -> 한번 매칭되는 case문을 실행하고 switch문을 빠져나온다.(C에는 break문을 사용하지만 스위프트는 필요 없다.)
        - `break`문은 옵션이다.
        - *case문에는 실행가능한 코드가 꼭 위치해야 한다.*
        - 비교되는 value값엔 반드시 같은 타입이 들어가야 한다.
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
        - 두개의 case에 대해 match 하고 싶다면 comma `,` 사용
            ```swift
            let anotherCharacter: Character = "a"
            switch anotherCharacter {
            case "a", "A": // 사용
                print("The letter A")
            default:
                print("Not the letter A")
            }
            // Prints "The letter A"
            ```
    - Interval Matching : 범위를 사용할 수 있다.
        ```swift
        let number = 5
        switch number {
        case 0:
            print("number 0")
        case 1...5:
            print(number)
        default:
            print("what number?")
        }
        ```
    - Tuples
        * 튜플을 사용하여 동일한 스위치문에서 여러 값을 테스트 할 수 있다.
        - 튜플의 요소는 서로 다른 값이나 값의 범위에 대해서 테스트할 수 있습니다. 어떤 값이든 매치시키려면 `_`와일드카드 패턴을 사용할 것.
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
            // Prints "(1, 1) is inside the box"
            ```
            - 위에서 somePoint를 (0, 0)으로 설정할 경우 모든 case에 일치하지만 일치하는 가장 첫번째 케이스만 실행된다.
    - Value Bindings
        - 임시적으로 case문 안에서 상수나 변수에 binding시킬 수 있다.
            ```swift
            let anotherPoint = (2, 0)
            switch anotherPoint {
            case (let x, 0):
                print("on the x-axis with an x value of \(x)")
            case (0, let y):
                print("on the y-axis with a y value of \(y)")
            case let (x, y):
                print("somewhere else at (\(x), \(y))")
            }
            // Prints "on the x-axis with an x value of 2"
            ```
            * 코드를 보면 deafult case가 없는데 이는 맨 마지막 case let(x, y)가 모든 case를 수용할 수 있기 때문이다.
    - Where
        - `where`을 사용해서 추가적인 조건을 검사할 수 있다.
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
            // Prints "(1, -1) is on the line x == -y"
            // 이전 예제와 같이 마지막 case가 나머지 경우에 대해 처리하므로 default case는 필요 없다.
            ```
            * switch case는 값에 대한 where 조건이 true로 평가될때만 일치하는 것으로 판단한다.
    - Compound Cases
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
        // Prints "e is a vowel"
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
* 5개 : `continue`, `break`, `fallthrough`, `return`, `throw` (여기선 continue, break, fallthrough만 설명, return은 Function챕터에서, throw는 Propagating Erros Using Throwing Functions챕터에서 다룬다.)
* Continue
    - continue를 만나면 바로 다음 반복을 실행한다.
* Break
    - 스위치문에서 break를 만나면 switch문을 탈출하거나 loop statment를 탈출
* Break in a Loop Statement
    - 루프문에서 break를 사용할 경우 루프 코드 블록{} 한개를 빠져나간다.
* Break in a Switch Statement
    - Switch문 탈출
* Fallthrough
    - C에서 break를 안쓰는것과 같은 효과
    - fallthrough 도 break처럼 만나면 다음코드를 실행하지 않고 바로 다음 case나 default block 으로 이동
    - Note: fallthrough를 사용하면 다음 case의 조건을 확인하지 않고 해당 case문의 내용을 실행시킨다.
* Labeled Statements
    - 반복문이나 조건문을 중첩으로 사용 할 때, `statement label`을 사용해서 어떤 반복문이나 조건문을 break, continue할지 명시하는 것.(조건문이나 반복문에다가 이름을 붙여주는것)
    - example: while문에 대한 label
        ```swift
        label name: while condition {
            statements
        }
        ```

#### Early Exit
`guard`, 항상 else문을 가지고 있음
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
* 옵셔널 바인딩을 사용할 수 있다. -> 나머지 코드블록 안에서 옵셔널 바인딩의 변수나 상수를 사용할 수 있다.
* 조건이 충족되지 않을때 else문 안에 있는 코드가 실행되는데, 이때 반드시 분기는 guard문이 있는 코드블록을 종료해야한다.(`return`, `break`, `continue`, `throw`, `fatalError(_:file:line:)`을 통해서)
* `guard`를 쓰면 코드의 가독성 증가(if에 비해서)
* else 블록에서 wrapping 없이 코드를 작성할 수 있게해주고, it lets you keep the code that handles a violated requirement next to the requirement.

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
