#### Terminology

#### Assignment Operator

#### Arithmetic Operators
* 스위프트에서의 Remainder Operator(나머지 연산)은 modulo연산과는 조금 다르다.(음수에서의 동작)

#### Compound Assignment Operators
* The compound assignment operators don't return a value.
    - `let b = a += 2` (X)

#### Comparison Operators
* 스위프트에서는 identity operators(=== and !==)를 지원한다.
    - 두 object references가 같은 object instance를 참조하고 있는지 확인
* Tuple의 비교
    - 두 개의 튜플이 같은 타입과 같은 개수를 가지고 있다면 비교가능
    - __왼쪽에서 오른쪽으로 한번에 하나씩 비교되고, 두 값이 같지 않을때 까지 비교__
    ```swift
    (1, "zebra") < (2, "apple")   // true, 1이 2보다 작다. zebra와 apple은 비교되지 않는다.
    (3, "apple") < (3, "bird")    // true, 3과 3은 같기 때문에 다음 element비교, bird가 apple보다 크므로
    (4, "dog") == (4, "dog")      // true, 4와 4가 같고 dog와 dog가 같다. 모든 element가 같을 경우
    ```
    - 튜플은 주어진 연산자가 각 요소에 적용될 수 있을 경우에만 주어진 연산자로 비교할 수 있다.
        - (String, Int)는 `<`로 비교 될 수 있지만 (String, Bool)은 `<`로 비교될 수 없다.
        ```swift
        ("blue", -1) < ("purple", 1) // OK, evaluates to true
        ("blue", false) < ("purple", true) // Error, < 로 Boolean값 비교 불가
        ```
    - 참고로 Swift 표준라이브러리에는 7개 미만의 요소가 있는 튜플에 대한 연산자만 있다.

#### Ternary Conditional Operator
```swift
let contentHeight = 40
let hasHeader = true
let rowHeight = contentHeight + (hasHeader ? 50 : 20)
print(rowHeight) // 90
```

#### Nil Coalescing Operator(nil 병합 연산자)
* unwrapping 과정이 포함되어 있다. 반환할 때 옵셔널을 unwrapping해서 반환한다.
* `a ?? b` : a가 nil이 아니면 unwrapped된 a를 반환, a가 nil이면 b를 반환(a는 항상 optional type), b는 a에 저장된 type과 일치해야한다.
* 두개의 코드는 같은 거
    ```swift
    a ?? b
    a != nil ? a! : b
    ```
* 만약에 a가 non-nil이라면 b의 value는 계산되지 않는다.(*short-circuit evaluation*)
    - 궁금해서 해보기
    ```swift
    var a: Int? = 1
    var b: Int = 7
    print(a ?? (b = 4 + 5))  // 1 출력
    print(b) // 7출력
    // ??뒤에 식이 어떻든 a가 nil이 아니기 때문에 print(b)의 결과는 그대로 7
    ```

#### Range Operators

#### Logical Operators
* 스위프트의 logical operators `&&`, `||`는 왼쪽부터 계산된다.
