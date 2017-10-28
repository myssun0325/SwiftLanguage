### 문자열 보간법
* 문자열 보간법을 통해 문자열로 치환되려면 변수나 상수타입이 `CustomStringConvertible` 프로토콜을 준수해야함.


#### Constants and Variables
  - Type Annotation(타입 지정, 변수 또는 상수의 이름 뒤에 콜론을 붙이고 타입을 명시하는 것)
    > var welcomeMessage: String  

  - Unicode
  - `print(_:separator:terminator:)`

#### Comments

#### Semicolons

#### Integers
  - 스위프트의 기본 데이터 타입은 구조체를 타입의 기반으로 구현되어 있음.
  - 32비트, 64비트 아키텍쳐에 따라서 Int타입이 Int32, Int64로 지정된다.
  - Int, UInt를 같이 사용하면 오히려 정수 타입의 변수끼리도 값을 교환할 때 많은 자원을 소모할 수 있으므로 UInt가 꼭 필요한 경우가 아니면 Int를 기본으로 사용할 것.
  - Integer Bounds : `min`, `max`

#### Floating-Point Numbers
  - double: 15자리, float: 6자리

#### Type Safety and Type Inference

#### Numeric Literals
  - 2진수 접두어 : `0b`, 8진수: `0o`, 16진수: `0x`
  - Floating-poin literal
      - decimal numbers: `e`, the base number is multiplied by 10^exp^
      > 1.25e2 = 1.25 x 10^2^, or 125.0
      > 1.25e-2 = 1.25 x 10^-2^, or 0.0125

      - hexadecimal numbers: `p`, the base number is multiplied by 2^exp^
      > 0xFp2 = 15 x 2^2^, or 60.0
      > 0xFp-2 = 15 x 2^-2^, or 3.75
      > 0xC.3p0 = 12.1875

#### Numeric Type Conversion
```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one) // UInt16타입의 2001
```
* `SomeType(ofInitialValue)` = default way to call the initializer of Swift type and initial value. (위에서 UInt16(one)을 예시로)
    - 사실 위에서 UInt16는 UInt8 값을 받을 수 있는 이니셜라이저를 가지고 있다. 그래서 위에서 UInt16는 UInt8의 값(one)으로부터 새로운 UInt16타입의 값을 만들어서 twoThousand와 더한것! (새로운 인스턴스 생성하여 할당)
* 리터럴은 `3 + 0.14`가 되는데 이것은 리터럴엔 명시적 type을 갖고있지 않기 때문에, 리터럴 타입은 컴파일러에 의해 evaluate될 때만 추론이 된다.


#### Type Aliases

#### Booleans
* Bool

#### Tuples
```swift
let http404Error = (404, "Not Found")
let (statusCode, statusMessage) = http404Error

print("The status code is \(statusCode)") // The status code is 404
print("The statue message is \(statusMessage)") // The statue message is Not Found
```
* tuple's values 중 몇개만 필요할 때 :
```swift
let (justTheStatusCode, _) = http404Error
print("The status code is \(justTheStatusCode)")
// The status code is 404
```
* index로 접근가능
> http404Error.0 // 404
> http404Error.1 // "Not Found"

* 각 element에 이름을 줄 수 있다.(접근도 요소 이름으로 가능)
> let http200Status = (statusCode: 200, description: "OK")
> print("The status code is \(http200Status.statusCode)")

* 튜플은 함수의 반환값으로써 사용할 때 유용하다.
    ```swift
    let possibleNumber = "123"
    let convertedNumber = Int(possibleNumber)
    // convertedNumber is inferred to be of type "Int?", or "optional Int"
    ```
    - Int타입의 이니셜라이저 중에 String값을 Int값으로 바꿀 수 있는 이니셜라이저가 있다. 이니셜라이저가 그냥 Int가 아니라 Optional Int를 반환하는데 이는 이 이니셜라이저가 실패할 수도 있기 때문에(실패하면 nil값을 반환)  

#### Optional
* 옵셔널 추출 : 포장을 뜯고 안에 있는 값에 접근
* Forced Unwrapping(강제추출)
    - 옵셔널이 값을 가지고 있지 않으면 (`nil`이라면) -> 런타임 오류
    - 보통 if optional != nil 조건을 활용해서 사용하지만 확실하지 않을 때 위험한 방법
* Optional Binding(옵셔널 바인딩)
    - 임시 상수나 변수를 활용하는 방법 -> 옵셔널 안에 nil이 아닌 값이 있는지 체크 후 값이 있을 경우 상수나 변수로 추출해서 사용한다(옵셔널 바인딩 = check + 추출).
    ```swift
    if let constantName = someOptional {
      statements
    }
    ```
* Implicitly Unwrapped Optionals(암시적 추출 옵셔널)
    - 처음 옵셔널이 정의된 직후에 옵셔널의 값이 존재하는 것으로 확인되고, 이후에 모든 시점에서 값이 존재한다고 가정할 수 있는 경우에 유용하게 사용
    - Swift에서 기본적인 암시적 추출 옵셔널 사용은 클래스를 초기화할 때 사용된다. (`Unowned References and Implicitly Unwrapped optional Properties`에 설명되어 있음)
      ```swift
      let possibleString: String? = "An optional string"
      let forcedString: String = possibleString! // requires an exclamation mark


      let assumedString: String! = "An implicitly unwrapped optional string"
      let implicitString: String = assumedString // no need for an exclamation mark

      print(type(of: assumedString))
      ImplicitlyUnwrappedOptional<String>
      ```
    - 암시적 추출 옵셔널은 일반 값처럼 사용할 수 있다.(옵셔널이기에 nil할당가능 -> 대신 런타임 오류)
    - 암시적 추출 옵셔널도 옵셔널은 옵셔널! -> 옵셔널 바인딩 당연히 사용 가능!

#### Error Handling
```swift
func canThrowAnError() throws {
  //this functino may or may not throw an error
}
```
* 오류가 발생한 여지가 있는 함수에 : `throws`
* 오류발생의 여지가 있는 함수를 호출할 때는 `try`를 사용하여 호출해야 한다.(try, try?, try!)
* throw는 `do-catch`문을 활용하여 오류처리
    * `do`는 새로운 스코프영역을 만든다. 이 영역에는 한 개 이상의 `catch`문으로 오류를 전달할 수 있다.
* 정리 코드
```swift
func makeASandwich() throws {
  // ...
}

do {
  try makeASandwich()
  eatASandwich()
} catch SandwichError.outOfCleanDishes {
  washDishes()
} catch SandwichError.missingIngredients(let ingredients) {
  buyGroceries(ingredients)
}

```

#### Assertions and Preconditions
  - 런타임때 발생하는 검사(디버깅 모드에서만 작동한다)
  - 더 많은 코드를 검사하기전에 필수 조건이 충족되었는지 확인하기 위해 사용
  - 주로 디버깅 중 조건의 검증을 위해 사용
  - Assertions : 디버그 빌드에서만 체크
  - preconditions : 디버그, production build에서 다 체크
  ```swift
  let age = -3

  if age > 10 {
      print("You can ride the roller-coaster or the ferris wheel.")
  } else if age > 0 {
      print("You can ride the ferris wheel.")
  } else {
      assertionFailure("A person's age can't be less than zero.")
  }
  ```
  - fatalError는 optimization setting과 상관없이 항상 실행중지
