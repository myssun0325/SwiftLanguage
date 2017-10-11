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

#### Closures Are Reference Types
* Escaping Closures
    - `self` (탈출클로저에서)
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

    completionHandlers.first?()
    print(instance.x)
    ```

#### Autoclosures
```swift
// customersInLine is ["Alex", "Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: { customersInLine.remove(at: 0) } )
// Prints "Now serving Alex!"
```
* autoclosure로
```swift
// customersInLine is ["Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: @autoclosure () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: customersInLine.remove(at: 0))
// Prints "Now serving Ewa!"
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
