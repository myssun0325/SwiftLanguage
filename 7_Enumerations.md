## Enumerations
* 스위프트에서 열거형은 연관된 항목들을 묶어서 표현할 수 있는 타입.
* Enumeration은 고유의 타입으로 인정(원시값이 필수는 아니다.)


#### Enumerations Syntax

#### Matching Enumeration Values with a Switch Statement

#### Associated Values

```swift

enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}

var productBarcode = Barcode.upc(1, 2, 3, 4)

productBarcode = .qrCode("ABCDEFGHIJK")

switch productBarcode {
case .upc(let numberSystem, let manufacterer, let product, let check):
    print("UPC: \(numberSystem), \(manufacterer), \(product), \(check)")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}


// 다 같은 경우 앞으로 뺄 수 있다.
switch productBarcode {
case let .upc(numberSystem, manufacterer, product, check):
    print("UPC: \(numberSystem), \(manufacterer), \(product), \(check)")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}


```

#### Raw Values

#### Recursive Enumerations
