## Methods
메서드는 특정 타입에 관련된 함수다. 클래스, 열거형, 구조체는 인스턴스 메서드와 타입메서드를 정의할 수 있다. 사용자가 만든 타입에도 메서드를 정의할 수 있다.

#### Instance Methods
특정 타입의 인스턴스에 속하는 함수. 인스턴스 메서드는 함수와 문법이 같다.
인스턴스 메서드는 암시적으로(implicit) 인스턴스의 다른 메서드나 프로퍼티의 접근할 수 있다.
*인스턴스 메서드는 자신이 속한 특정 타입의 인스턴스에서만 호출될 수 있다. 인스턴스가 존재하지 않으면 호출 할 수 없다.*

* The self Property
모든 타입의 인스턴스는 암시적 프로퍼티로 self를 가지고 있다. self는 인스턴스 자기 자신과 같다. self는 매개변수의 이름과 프로퍼티의 이름이 같을 때 유용하게 사용할 수 있다.
    ```swift
    struct Point {
        var x = 0.0, y = 0.0
        func isToTheRightOf(x: Double) -> Bool {
            return self.x > x
        }
    }
    let somePoint = Point(x: 4.0, y: 5.0)
    if somePoint.isToTheRightOf(x: 1.0) {
        print("This point is to the right of the line where x == 1.0")
    }
    // Prints "This point is to the right of the line where x == 1.0"
    ```
* Modifying Value Types from Within Instance Methods
    구조체와 열거형은 값타입이기 때문에 기본적으로 인스턴스메서드에서 값 타입의 내부 프로퍼티를 바꿀 수 없다.
    만약에 열거형이나 구조체의 특정 메서드에서 프로퍼티의 값을 바꾸려고 하면 `mutating`을 명시해줘야 한다.(명시하지 않을 경우 에러 발생)
    ```swift
    struct Point {
        var x = 0.0, y = 0.0
        mutating func moveBy(x deltaX: Double, y deltaY: Double) {
            x += deltaX
            y += deltaY
        }
    }
    var somePoint = Point(x: 1.0, y: 1.0)
    somePoint.moveBy(x: 2.0, y: 3.0)
    print("The point is now at (\(somePoint.x), \(somePoint.y))")
    // Prints "The point is now at (3.0, 4.0)"
    ```
    - 상수에는 mutating 메서드를 호출할 수 없다.
        ```swift
        let fixedPoint = Point(x: 3.0, y: 3.0)
        fixedPoint.moveBy(x: 2.0, y: 3.0)
        // this will report an error
        ```
* Assigning to self Within a Mutating Method
    - Mutating 메서드는 암시적 self property에 완전히 새로운 인스턴스를 할당할 수 있다.
    - 위의 예제와 동일한 동작
          ```swift
          // 위 예제를 변경
          struct Point {
              var x = 0.0, y = 0.0
              mutating func moveBy(x deltaX: Double, y deltaY: Double) {
                  self = Point(x: x + deltaX, y: y + deltaY)
              }
          }
          ```
    - 열거형에서 self
        ```swift
        enum TriStatewitch {
            case off, low, high
            mutating func next() {
                switch self {
                case .off:
                    self = .low
                case .low:
                    self = .high
                case .high:
                    self = .off
                }
            }
        }

        var ovenLight = TriStatewitch.low
        ovenLight.next()
        ovenLight.next()

        ```

#### Type Methods
타입자체에서 호출할 수 있는 타입메서드. `static`키워드사용. `static`대신 `class`를 사용하면 서브클래스에서 재정의할 수 있다.
타입메서드에서는 self는 인스턴스가 아닌 타입 자체를 가리킨다.
*
* __구조체와 열거형에서 타입메서드는 앞에 타입이름 없이 타입프로퍼티의 이름을 사용해서 타입프로퍼티에 접근할 수 있다.__ 왜? 암시적으로 self가 타입자체를 가르키기때문에, 사실 self는 생략되었다는 것을 이전 챕터에서 다뤘다.
* 타입메서드는 다른 타입메서드를 타입명시 없이 호출할 수 있다.
* 아래 `LevelTracker`구조체에서  `highestunlockedLevel`은 타입프로퍼티임에도 불구하고 타입메서드인 `unlock`이나 `isUnlocked`에서 타입이름 없이 바로 프로퍼티에 접근했다.
```swift
struct LevelTracker {
    static var highestUnlockedLevel = 1
    var currentLevel = 1

    static func unlock(_ level: Int) {
        if level > highestUnlockedLevel { highestUnlockedLevel = level }
    }

    static func isUnlocked(_ level: Int) -> Bool {
        return level <= highestUnlockedLevel
    }

    @discardableResult
    mutating func advance(to level: Int) -> Bool {
        if LevelTracker.isUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}

class Player {
    var tracker = LevelTracker()
    let playerName: String
    func complete(level: Int) {
        LevelTracker.unlock(level + 1)
        tracker.advance(to: level + 1)
    }
    init(name: String) {
        playerName = name
    }
}

var player = Player(name: "Argyrios")
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")

player = Player(name: "Beto")
if player.tracker.advance(to: 6) {
    print("palyer is now on level 6")
} else {
    print("level 6 has not yet been unlocked")
}
```
