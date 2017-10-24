## SubScripts

클래스, 구조체, 열거형 모두 서브스크립트를 정의할 수 있다.
서브스크립트는 collection, list, or sequence의 멤버요소에 접근하기 위한 shourtcut이다.
서브스크립트는 인덱스를 통해 값을 설정하거나 retrieve할 수 있다. 설정하거나 retrieve를 위한 별도의 메서드가 필요 없다.
예를 들어, 배열의 인스턴스에서 `someArray[index]`를 가져오거나 딕셔너리 인스턴스에서 `someDitionary[key]` 같은 것이다.

한 타입에 여러 개의 서브스크립트를 지정할 수 있다. 적절한 서브스크립트를 overload 할 수도 있다.

* Subscript Syntax
  - `subscript`키워를 사용한다.
  - 인스턴스 메서드와 다르게 서브스크립트는 읽기쓰기 또는 읽기 모드만 가능하다.
  - 연산 프로퍼티랑 비슷하다.
  - 형식:
      ```swift
      subsciript(index: Int) -> Int {
        get {
          // return an appropriate subscript value here
        }
        set(newValue) {
          // perform a suitable setting action here
        }
      }
      ```
  - read-only
      ```swift
      subscript(index: Int) -> Int {
        // return an appropriate subscript value here
      }
      ```

#### Subscript Usage
서브스크립트의 정확한 해석은 문맥에 의존한다. 서브스크립트는 보통 멤버요소에 접근하기 위한 shortcut으로 사용된다. 특정 클래스 또는 구조의 기능에 맞게 가장 적합한 방식으로 스크립트를 자유롭게 구현할 수 있다.
* 예를 들어, 딕셔너리 타입은 서브스크립트를 set, retrieve를 모두 구현한다. 서브스크립트를 통해 값을 가져올 수도 설정할 수도 있다.
    ```swift
    var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
    numberOfLegs["bird"] = 2
    ```
    * 참고로 스위프트의 딕셔너리 타입은 key-value 서브스크립트로 구현되어 있는데 반환타입이 옵셔널이다.(이유는 생각해보면 참 쉽다.)
    
