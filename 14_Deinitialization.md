## Deinitialization
디이니셜라이저는 클래스 인스턴스가 할당해제되기 바로 직전에 호출된다. `deinit`키워드 사용해서 구현한다. 디이니셜라이저는 클래스타입에서만 사용가능하다.

#### How Deinitialization Works
스위프트는 자원을 해제하기 위해 자동으로 인스턴스를 할당해제 된다. 스위프트는 ARC(automatic reference counting)을 통해 인스턴스의 메모리를 관리한다. 그러므로 인스턴스가 할당해제 될 때 수동으로 cleanup 할 필요가 없다.
However, when you are working with your own resources, you might need to perform some additional cleanup yourself. For example, if you create a custom class to open a file and write some data to it, you might need to close the file before the class instance is deallocated.

클래스는 많아봐야 한개의 디이셜라이저를 가지고 있다. 디이니셜라이저는 어떤 매개변수도 갖지 않는다.
```swift
deinit {
  // perform the deinitialization
}
```
디이니셜라이저는 자동으로 호출되기 때문에 임의로 호출할 수 가 없다.
부모클래스의 디이니셜라이저는 자식클래스로 상속되고, 부모클래스의 디이니셜라이저는 자식클래스의 이니셜라이저 구현의 마지막에 자동으로 호출된다. 자식클래스가 디이니셜라이저를 제공하지 않아도 부모클래스의 디이니셜라이저는 항상 호출된다.

인스턴스는 디이니셜라이저가 호출된 이후에 할당해제 되기 때문에, 디이니셜라이저는 인스턴스의 프로퍼티에 접근할 수 있다.

```swift
class Bank {
    static var coinsInBank = 10_000
    static func distribute(coins numberOfCoinsRequested: Int) -> Int {
        let numberOfCoinsToVend = min(numberOfCoinsRequested, coinsInBank)
        coinsInBank -= numberOfCoinsToVend
        return numberOfCoinsToVend
    }

    static func receive(coins: Int) {
        coinsInBank += coins
    }
}

class Player {
    var coinsInPurse: Int
    init(coins: Int) {
        coinsInPurse = Bank.distribute(coins: coins)
    }

    func win(coins: Int) {
        coinsInPurse += Bank.distribute(coins: coins)
    }

    deinit {
        Bank.receive(coins: coinsInPurse)
    }
}

var playerOne: Player? = Player(coins: 100)
print("A new player has joined the game with \(playerOne!.coinsInPurse) coins")
// Prints "A new player has joined the game with 100 coins"
print("There are now \(Bank.coinsInBank) coins left in the bank")
// Prints "There are now 9900 coins left in the bank"

playerOne!.win(coins: 2_000)
print("PlayerOne won 2000 coins & now has \(playerOne!.coinsInPurse) coins")
// Prints "PlayerOne won 2000 coins & now has 2100 coins"
print("The bank now only has \(Bank.coinsInBank) coins left")
// Prints "The bank now only has 7900 coins left"

playerOne = nil
print("PlayerOne has left the game")
// Prints "PlayerOne has left the game"
print("The bank now has \(Bank.coinsInBank) coins")
// Prints "The bank now has 10000 coins"

```
예제 해석가능
