## SubScripts

클래스, 구조체, 열거형 모두 서브스크립트를 정의할 수 있다.
서브스크립트는 collection, list, or sequence의 멤버요소에 접근하기 위한 shourtcut이다.
서브스크립트는 인덱스를 통해 값을 설정하거나 retrieve할 수 있다. 설정하거나 retrieve를 위한 별도의 메서드가 필요 없다.
예를 들어, 배열의 인스턴스에서 `someArray[index]`를 가져오거나 딕셔너리 인스턴스에서 `someDitionary[key]` 같은 것이다.

한 타입에 여러 개의 서브스크립트를 지정할 수 있다. 적절한 서브스크립트를 overload 할 수도 있다.

#### Subscript Syntax
  - `subscript`키워를 사용한다.
  - 인스턴스 메서드와 다르게 서브스크립트는 읽기쓰기 또는 읽기 모드만 가능하다.
  - 연산 프로퍼티랑 비슷하다.
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

#### Subscript Options
* 서브스크립트는 입력 매개변수를 몇개든지 가질 수 있다. 이 input parameter는 어떤 타입이여도 상관없다.
* 서브스크립트는 어떤 타입이든 반환할 수도 있다.
* 서브스크립트는 `가변매개변수`를 사용할 수 있지만, in-out parameter를 사용할 수 없고, 매개변수 기본값(default parameter values)도 사용할 수 없다.
* 서브스크립트는 중복정의(overloading)이 가능하다.
* 파라미터를 하나만 사용하는게 일반적이지만 파라미터를 여러개 사용할 수 있다.
```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }
    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && columns >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}

var matrix = Matrix(rows: 2, columns: 2)

matrix[0, 1] = 1.5
matrix[1, 0] = 3.2

//let someValue = matrix[2, 2]
// trigger an assert, outside of the matrix bounds
```
