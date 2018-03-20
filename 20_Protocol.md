## Protocol

> 특정 역할을 하기 위한 메서드, 프로퍼티, 기타 요구사항 등의 청사진. 어디서 무엇을 상속받느냐가 아니라 ___무엇을 할 수 있는가?___ 에 초점을 맞춘다. 정의를 하고 제시를 할 뿐 스스로 기능을 구현하진 않는다.

> 어떤 타입이든 requirements of protocol을 만족하면 그 프로토콜을 ___conform___ 한다고 표현한다.

#### Protocol Syntax

```swift
protocl SomeProtocol {
	// protocol definition goes here
}
```

```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // structure definition goes here
}
```

```swift
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // class definition goes here
}

```


#### Property Requirements
* 프토로콜은 프로퍼티의 종류(연산 프로퍼티 or 저장 프로퍼티)인지 제시하지 않는다. 단지 이름과 타입만 요구한다.
* 만약에 프로토콜이 프로퍼티를 gettable 또는 settable로 요구하면, 상수나 읽기전용 연산 프로퍼티로 구현할 수 없다.
* 프로토콜이 gettable로만 요구를 한다면, 어떤 종류의 프로퍼티로 구현하든 상관없습니다. 
* 프로토콜의 프로퍼티 요구사항은 항상 변수 프로퍼티로 선언합니다. 

```swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```

* 타입 프로퍼티를 요구하려면 `static` 을 사용합니다. 상속 가능한 class타입 프로퍼티와 상속 불가능한 static 타입 프로퍼티 모두 따로 구분하지 않고 static 키워드를 사용합니다.

* 프토토콜이 프로퍼티를 gettable로 요구할 경우 get,set 상관없는 예

```swift
protocol FullyNamed {
    var fullName: String { get }
}

struct Person: FullyNamed {
    var fullName: String
}
let john = Person(fullName: "John Appleseed")
// john.fullName is "John Appleseed"

print(john.fullName)

class Starship: FullyNamed {
    var prefix: String?
    var name: String
    init(name: String, prefix: String? = nil) {
        self.name = name
        self.prefix = prefix
    }
    var fullName: String {
        return (prefix != nil ? prefix! + " " : "") + name
    }
}
var ncc1701 = Starship(name: "Enterprise", prefix: "USS")
// ncc1701.fullName is "USS Enterprise"
print(ncc1701.fullName)
```


### Method Requirements
> 프토로콜은 특정 인스턴스 메서드나 타입 메서드를 요구할 수도 있습니다. 메서드는 curly brace나 method body를 쓰지 않습니다.

