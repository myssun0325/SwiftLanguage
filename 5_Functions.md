#### Defining and Calling Functions

#### Function Parameters and Return Values
* Functions Without Parameters
    ```swift
    // 파라미터가 없어도 괄호는 항상 있어야한다.
    func sayHello() -> String {
      return "hello, world"
    }
    ```
* Functions With Multiple Parameters
    ```swift
    func greet(person: String) -> String {
      return "Hello, " + person + "!"
    }

    func greet(person: String, alreadyGreeted: Bool) -> String {
      if alreadyGreeted {
        return greetAgain(person: person)
      } else {
        return greet(person: person)
      }
    }
    print(greet(person: "Tim", alreadyGreeted: true))
    ```
    - 이름이 같아도 매개변수가 다르다 -> overload(오버로드, 중복정의) 둘은 다른 함수
* Functions Without Return Values
    ```swift
    func greet(person: String) {
      print("Hello, \(person)")
    }
    ```
    - *사실 return하는 값이 없어보이지만 실제는 Void타입을 리턴한다.(empty tuple`()`을 리턴)*
    - 참고로 이전 과제를 하면서 알게 된 사실 : 파라미터가 아니라 반환값에 따른 오버로드도 또한 가능!
    - NOTE: Return values can be ignored, but a function that says it will return a value must always do so. A function with a defined return type cannot allow control to fall out of the bottom of the function without returning a value, and attempting to do so will result in a compile-time error.
* Functions with Multiple Return Values
    - tuple을 return type으로 사용
    ```swift
    func minMax(array: [Int]) -> (min: Int, max: Int) {
        var currentMin = array[0]
        var currentMax = array[0]
        for value in array[1..<array.count] {
            if value < currentMin {
                currentMin = value
            } else if value > currentMax {
                currentMax = value
            }
        }

        return (currentMin, currentMax)
    }

    let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
    print("min is \(bounds.min) and max is \(bounds.max)")
    // bounds.min, bounds.max에서 튜플의 멤버 값은 함수의 반한 타입의 부분으로 이름이 붙여진다.
    // 튜플의 멤버는 이름을 따로 지정해주지 않아도 된다.
    ```
* Optional Tuple Return Types
    - __주의 : (Int, Int)? 은 튜플 전체(자체)가 옵셔널인 거고, (Int?, Int?)는 각각 튜플의 요소(값이)가 옵셔널이다__
    - 위 예제에서 함수는 넘겨진 array에 대해서 어떠한 safety check도 하지 않는다. 만약에 넘겨받은 array가 비어있다면 runtime에러가 발생한다. 이를 다루기 위해서 *optional tuple return type* 을 활용
    ```swift
    func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
    }   

    // 이 후 minMax(array:)함수가 튜플을 반환하는지 nil을 반환하는지 체크하기 위해 옵셔널 바인딩 사용
    if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
    print("min is \(bounds.min) and max is \(bounds.max)")
    }

    ```

#### Function Argument Labels and Parameter Names
* `func someFunction(argumentLabel parameterName: Int) { 안에서는 parameterName으로 사용, 호출할 땐 argementLabel로 사용 }`
    ```swift
    func greet(person: String, from hometown: String) -> String {
    return "Hello \(person)!  Glad you could visit from \(hometown)."
    }
    print(greet(person: "Bill", from: "Cupertino"))
    ```
* Omitting Argument Labels
    ```swift
    func someFunction(_ firstParamterName: Int, secondParameterName: Int) {
      ...
    }
    someFunction(1, secondParameterName: 2)
    ```
* Default Parameter Values
    ```swift
    func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
      // If you omit the second argument when calling this function, then
      // the value of parameterWithDefault is 12 inside the function body.
    }
    // 호출하기
    someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault is 6
    someFunction(parameterWithoutDefault: 4) // parameterWithDefault is 12
    ```
    - 매개변수 목록의시작 부분에 기본값이 없는 매개변수를 기본값이 있는 매개변수 앞에 배치할 것(기본값이 없는 매개변수의 함수의 의미상 더 중요하다)
* Variadic Parameters(가변 매개변수)
    - 가변매개변수로 들어온 인자 값은 배열처럼 사용할 수 있다.
    ```swift
    func arithmeticMean(_ numbers: Double...) -> Double {
        var total: Double = 0
        for number in numbers {
            total += number
        }

        return total / Double(numbers.count)
    }

    arithmeticMean(1, 2, 3, 4, 5)
    // returns 3.0, which is the arithmetic mean of these five numbers
    arithmeticMean(3, 8.25, 18.75)
    // returns 10.0, which is the arithmetic mean of these three numbers
    ```
    - 가변매개변수는 0개 이상의 값을 받아올 수 있고, 함수는 최대 하나의 가변매개변수만 가질 수 있다.
* In-Out Parameters
함수의 매개변수는 기본적으로 상수다. 그래서 함수의 매개변수의 값을 함수안에서 변경하려고하면 compile-time error가 발생한다.
    - 함수가 매개변수의 값을 변경하고, 함수 호출이 끝난 후에도 그 변화를 유지하려면 매개변수를 `in-out parameter`로 정의하면 된다.
    - 매개변수 타입 앞에 `inout`키워드 사용
    - 내부적으로 In-Out paramter의 전달순서 (*copy-in copy-out*)
        1. When the function is called, the value of the argument is copied.
        2. In the body of the function, the copy is modified.
        3. When the function returns, the copy’s value is assigned to the original argument.
        - 만약에 연산프로퍼티나 프로퍼티 감시자를 가지는 프로퍼티가 입출력 매개변수로 전달될 때, 함수가 호출될 때 getter가호출되고, 함수가 종료되고 반환될 때 setter가 호출된다.
    - in-out 매개변수로는 변수만 넘길 수 있다.(상수나 literal value는 불가능)
    - in-out 매겨변수로 넘겨주는 인자는 ampersand를 붙인다. `&`
    - in-out parameter는 기본 값을 가질 수 없고, 가변 매개변수는 inout으로 표시할 수 없다.

    ```swift
    func swapTwoInts(_ a: inout Int, _ b: inout Int) {
        let temporaryA = a
        a = b
        b = temporaryA
    }

    var someInt = 3
    var anotherInt = 107
    swapTwoInts(&someInt, &anotherInt)

    print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
    // someInt is 107, anotherInt is 3
    ```

#### Function Types
* `func printHelloWorld() { print("hello, world") }` : type is `() -> Void`
* Using Function Types
    > var mathFunction: (Int, Int) -> Int = addTwoInt(임의의 함수, 왼쪽의 타입에 맞는)

* Function Types as Parameter Types
    - 함수를 파라미터로 받음
    ```swift
    func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
    }

    func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
    }
    printMathResult(addTwoInts, 3, 5)
    // Prints "Result: 8"
    ```
* Function Types as Return Types
```swift
func stepForward(_ input: Int) -> Int {
  func stepForward(_ input: Int) -> Int {
      return input + 1
  }
  func stepBackward(_ input: Int) -> Int {
      return input - 1
  }

  func chooseStepFunction(backward: Bool) -> (Int) -> Int {
      return backward ? stepBackward : stepForward
  }
  var currentValue = 3
  let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
  // moveNearerToZero now refers to the stepBackward() function

  print("Counting to zero:")
  while currentValue != 0 {
      print("\(currentValue)...")
      currentValue = moveNearerToZero(currentValue)
  }

  print("zero!")

```

#### Nested Functions
```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```
