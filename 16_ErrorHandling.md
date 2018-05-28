## Error Handling

> Error handling is the process of responding to and recovering from error conditions in your program.
> 오류처리는 프로그램에서 오류가 발생했을 때 이에 반응하고 회복시키는 일련의 과정.

스위프트에서 오류는 Error라는 프로토콜을 준수하는 타입의 값을 통해 표현한다.  
Error 프로토콜은 사실상 요구사항이 없는 빈 프로토콜이지만 오류를 표현하기 위해 타입(열거형)은 이 프로토콜을 채택.

```swift
enum VendingMachineError: Error {
	case invalidSelection
	case insufficientFunds(coinsNeeded: Int)
	case outOfstock
}
```


* 오류를 던짐

	```swift
	throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
	```
	
* 스위프트의 오류를 처리하는 4가지 방법
	1. 함수에서 발생한 오류를 해당 함수를 호출한 코드에 알리는 방법
	2. do-catch구문을 써서 오류를 처리하는 방법
	3. 옵셔널 값으로 오류를 처리하는 방법
	4. 오류가 발생하지 않을 것이라고 확신하는(assert)하는 방법
	
1. Propagating Errors Using Throwing Functions
	- 함수, 메서드, 이니셜라이저 뒤에 throw 키워드를 사용하여 오류를 던질 수 있다. throw 키워드가 표시된 함수는 throwing function라고 합니다.
	
	```swift
	func canThrowErros() thorws -> String
	```
	
	- thorws는 함수나 메서드의 타입에도 영향을 미친다. throws는 같은 이름의 throws가 명시되지 않은 함수나 메서드와 구분된다. throws를 포함한 함수, 메서드, 이니셜라이저는 일반함수, 메서드, 이니셜라이저로 재정의할 수 없다. 하지만 반대는 가능.
	- 오류를 던지는 함수를 호출하는 메서드 또는 함수도 오류를 던질 수 있어야 하므로 throws키워드를 통해 오류를 던질 수 있는 함수로 구현해줘야 한다.
	- try는 시도만 할 뿐!

2. do-catch구문을 써서 오류 잡기
	- 오류를 던저주면 오류 발생을 전달받은 코드 블록은 `do-catch`를 사용하여 오류를 처리해 준다.
	
3. 옵셔널 값으로 오류를 처리하기

```swift

import Foundation

func someThrowingFunction(shouldThrowError: Bool) throws -> Int {
    if shouldThrowError {
        enum SomeError: Error {
            case justSomeError
        }
        
        throw SomeError.justSomeError
    }
    
    return 100
}

let x: Optional = try? someThrowingFunction(shouldThrowError: true)
print(x)	// nil
let y: Optional = try? someThrowingFunction(shouldThrowError: false)
print(y)	// Optional(100)
```

- 함수의 반환값이 Int더라도 `try?`표현을 사용하면 반환타입이 옵셔널이 된다.

```swift
func fetchData() -> Data? {
    if let data = try? fetchDataFromDisk() {
        return data
    }
    
    if let data = try? fetchDataFromServer() {
        return data
    }
    
    return nil
}
```

- 옵셔널 값으로 오류를 처리한느 방법과 기존 옵셔널 반환타입의 결합

4. 오류가 발생하지 않을 것이라고 확신하는(assert)하는 방법
	- try! : 오류가 발생하면 런타임 오류를 발생시키고 프로그램 강제 종료
	

- 이 외에 defer와 다시던지기