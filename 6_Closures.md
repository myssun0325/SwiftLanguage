클로저 부분은 어려운 부분이므로 스위프트 프로그래밍[야곰 저]와 함께 여러번 보고 이해할 것

#### Closures
* 3개 중 하나의 모양을 갖는다.
    - 전역함수(Global functions)의 형태 : 이름을 갖고, 어떤 값도 획득하지 않는다.
    - 중첩함수(Nested functions)의 형태 : 이름을 갖고, 내부 함수(enclosing function)으로 부터 값을 캡처
    - 클로저 표현식(Closure expressions) : 이름을 갖지 않고(간단한 구문), 주변문맥으로부터 값을 캡처
* 클로저의 최적화는 다음을 포함
    - (Inferring parameter and return value types from context)문맥(context)으로부터 파라미터와 반환 값 타입을 추론
    - single-expression closure(단일 표현 클로저)에서 암시적인 반환
    - shorthand argument names
    - trailing closure syntax(후행 클로저 문법)

#### Closure Expressions(클로저 표현식)
* The Sorted Method
    - `sorted(by:)`메서드는 내가 제공하는 클로저의 결과물을 기반으로 배열의 요소를 정렬한다.
    ```swift
    let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]

    func backward(_ s1: String, _ s2: String) -> Bool {
        return s1 > s2
    }

    var reversedNames = names.sorted(by: backward)
    ```
* Closure Expression Syntax
    - 클로저 표현식 문법
    ```swift
    { (parameters) -> return type in
        statements
    }
    ```
    - 클로저 표현식 안에서 매개변수는 in-out매개변수가 될 수 있지만, 기본값은 가질 수 없다
    - 가변 매개변수 이름을 지정하면 가변매개변수를 사용할 수 있다.
    - 튜플은 매개변수와 반환 타입 전부 사용 가능
    - closure expression version
        ```swift
        reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
            return s1 > s2
        })
        ```
* Inferring Type From Context
    ```swift
    // 어차피 메소드에 인자로 전달되는거기 때문에 매개변수의 타입과 반환타입은 메소드에 달려있다. 메소드에서 받는 게 (String, String) -> Bool 타입함수이기 때문에 추론가능하여 생략가능
    reversedNames = names.sorted(by: {s1, s2 in return s1 > s2 })
    ```
    - 즉, 클로저가 함수나 메소드를 inline closure expression으로 전달할 때 매개변수타입이나 반환 타입은 언제나 생략 가능

* Implicit Returns from Single-Expression Closures
> reversedNames = names.sorted(by: { s1, s2 in s1 >  s2 } )

* Shorthand Argument Names
> reversedNames = names.sorted(by: { $0 > $1 } )

* Operator Methods
> reversedNames = names.sorted(by: >)

#### Capturing Values
* 클로저는 정의된 주변 컨텍스트로부터 변수나 상수를 획득(capture)할 수 있다. 그 다음 클로저는 이런 변수나 상수가 더 이상 존재하지 않더라도 이를 참조하거나 수정할 수 있다.
* 스위프트에서 값을 획득할 수 있는 가장 간단한 형태는 중첩함수(nested function)다. nested function은 외부함수의 인자(argument)를 획득할 수 있으며 외부 함수내에 정의된 어떤 상수나 변수라도 획득할 수 있다.
    ```swift
    func makeIncrementer(forIncrement amount: Int) -> () -> Int {
        var runningTotal = 0
        // nested function
        func incrementer() -> Int {
            // capture values : runningTotal, amount from its surrounding context.
            runningTotal += amount
            return runningTotal
        }
        // After capturing 2 values, incrementer is returned by makeIncrementer as a closure
        return incrementer
    }

    let incrementByTen = makeIncrementer(forIncrement: 10)

    incrementByTen()
    // returns a value of 10
    incrementByTen()
    // returns a value of 20
    incrementByTen()
    // returns a value of 30

    let incrementBySeven = makeIncrementer(forIncrement: 7)
    incrementBySeven()
    // returns a value of 7

    incrementByTen()
    // returns a value of 40
    ```
    - Note : 스위프트는 만약에 value가 클로저에 의해 바뀌지 않는다면, value의 복사본을 대신 획득하고 저장할 수도 있다.  
    - 원문 : As an optimization, Swift may instead capture and store a copy of a value if that value is not mutated by a closure, and if the value is not mutated after the closure is created. Swift also handles all memory management involved in disposing of variables when they are no longer needed.
    - Note : If you assign a closure to a property of a class instance, and the closure captures that instance by referring to the instance or its members, you will create a strong reference cycle between the closure and the instance. Swift uses capture lists to break these strong reference cycles. For more information, see Strong Reference Cycles for Closures.(강한 참조 순환 문제에 관한 내용)


#### Closures Are Reference Types
위 예제에서 `incrementBySeven`과 `incrementByTen`은 상수지만, 이 상수들이 참조하는 클로저는 여전히 획득한 runningTotal을 증가시킬 수 있다. 왜냐하면 함수와 클로저는 참조타입(reference type)이기 때문이다. 함수나 클로저를 상수나 변수에 할당하는 것은 실제로 참조를 세팅하는 것이다. 클로저의 내용을 할당하는 것이 아니라 incrementByTen이 참조하는 클로저를 선택하는 것이다.
* 두개의 다른 변수나 상수에 클로저를 할당하면 두 상수나 변수는 같은 클로저를 참조하게 된다.
> let alsoIncrementByTen = incrementByTen
  alsoIncrementByTen()

* Escaping Closures
    - 클로저가 함수의 전달인자로 넘겨지고 나서 함수가 종료되고 클로저가 호출될 때 클로저가 함수를 탈출(escape)한다고 말한다.
    - 함수의 매개변수로 탈출클로저를 사용해야할 때 `@escaping`키워드를 매개변수 타입 앞에 쓸 것.
    - 클로저가 탈출 할 수 있는 한가지 방법은 클로저가 함수 외부에 정의되어 있는 함수에 저장되는 것이다.
    - 비동기 작업(asynchronous operation)을 시작하는 많은 함수들이 전달인자로 클로저를 받고 이 클로저를 completion handler로 사용한다. 비동기 작업을 시작한후에 함수는 return(종료)하지만, 클로저는 비동기작업이 완료될때까지 호출되지 않는다.(-> 함수가 종료된 후 비동기 작업이 완료되면 호출되는 경우이므로 탈출이 필요하다.)
    - `@escaping`을 표시하는 것은 클로저 안에서 `self`를 명시적으로 참조해야 한다는 것을 의미한다.
    - 아래 예제에는 탈출 클로저는 self를 명시했고, 비탈출 클로저는 명시하지 않았다.(비탈출 클로저는 self를 암시적으로(implicitly)참조하고 있다.)
        ```swift
        var completionHandlers: [() -> Void] = []
        func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
            completionHandlers.append(completionHandler)
        }

        func someFunctionWithNonescapingClosure(closure: () -> Void) {
            closure()
        }

        class SomeClass {
            var x = 10
            func doSomething() {
                someFunctionWithEscapingClosure { self.x = 100 }
                someFunctionWithNonescapingClosure { x = 200 }
            }
        }

        let instance = SomeClass()
        instance.doSomething()
        print(instance.x)
        // Prints "200"

        completionHandlers.first?()
        print(instance.x)
        // Prints "100"

        ```

#### Autoclosures
* auto클로저를 설정해주면 클로저가 아닌 코드의 결과가 전달인자로 넘어간다. 아래 예제에서 String 타입의 문자열이 전달인자로 넘어가게 되는데, 자동클로저 매개변수에 String타입이 전달받게 되면 *String값을 매개변수가 없고 String값을 반환하는 클로저로 변환해준다.* String 타입의 값을 전달받는 이유는 자동클로저의 반환 타입이 String이기 때문이다.
    ```swift
    var customersLine: [String] = ["Y", "S", "H", "M"]

    let customerProvider: () -> String = {
        return customersLine.removeFirst()
    }


    func serveCustomer(_ customerProvider: () -> String) {
        print("Now serving \(customerProvider())!")
    }
    serveCustomer( { customersLine.removeFirst() } )
    // Now serving Y!

    // 자동클로저 사용
    func serveCustomer(_ customerProvider: @autoclosure () -> String) {
        print("Now serving \(customerProvider())!")
    }
    serveCustomer(customersLine.removeFirst())
    // Now serving S!

    ```
* 자동클로저의 escaping
    ```swift
    var customersInLine: [String] = ["Barry", "Daniella"]
    var customerProviders: [() -> String] = []
    func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
        customerProviders.append(customerProvider)
    }
    collectCustomerProviders(customersInLine.remove(at: 0))
    collectCustomerProviders(customersInLine.remove(at: 0))

    print("Collected \(customerProviders.count) closures.")
    // Prints "Collected 2 closures."
    for customerProvider in customerProviders {
        print("Now serving \(customerProvider())!")
    }
    // Prints "Now serving Barry!"
    // Prints "Now serving Daniella!"

    ```

* autoclosure의 escaping
```swift
// customersInLine is ["Barry", "Daniella"]
var customerProviders: [() -> String] = []
func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
    customerProviders.append(customerProvider)
}
collectCustomerProviders(customersInLine.remove(at: 0))
collectCustomerProviders(customersInLine.remove(at: 0))

print("Collected \(customerProviders.count) closures.")
// Prints "Collected 2 closures."
for customerProvider in customerProviders {
    print("Now serving \(customerProvider())!")
}
// Prints "Now serving Barry!"
// Prints "Now serving Daniella!"
```
