# Smart\_Overflow

Bài yêu khai thác bug **integer overflow** trong contract Solidity

Contract:

```
pragma solidity ^0.6.12;

contract IntOverflowBank {
    mapping(address => uint256) public balances;
    address public owner;
    string private flag;
    bool public revealed;

    event Deposit(address indexed who, uint256 amount);
    event Withdraw(address indexed who, uint256 amount);
    event FlagRevealed(string flag);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner");
        _;
    }

    constructor() public {
        owner = msg.sender;
        revealed = false;
    }

    function setFlag(string memory _flag) external onlyOwner {
        flag = _flag;
    }

    function deposit(uint256 amount) external {
        uint256 oldBalance = balances[msg.sender];
        balances[msg.sender] = balances[msg.sender] + amount;

        emit Deposit(msg.sender, amount);
        if (!revealed && balances[msg.sender] < amount) {
            revealed = true;
            emit FlagRevealed(flag);
        }
    }

    function withdraw(uint256 amount) external {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] = balances[msg.sender] - amount;
        emit Withdraw(msg.sender, amount);
    }

    function getFlag() external view returns (string memory) {
        require(revealed, "Flag not revealed yet");
        return flag;
    }
}
```

Trong contract logic là:

```
balances[msg.sender] = balances[msg.sender] + amount;

if (!revealed && balances[msg.sender] < amount) {
    revealed = true;
    emit FlagRevealed(flag);
}
```

Muốn condition đúng:

```
balances[msg.sender] < amount
```

⇒ Phải **overflow sau khi đã có balance trước đó** (do contract ko ktra overflow)**.** Nếu **overflow xảy ra**, giá trị mới sẽ **nhỏ hơn amount**

**-> overflow + deposit 2 lần**

Ví dụ:

```
balance = 0
amount = 2^256 - 1
```

Sau phép cộng:

```
0 + (2^256 - 1) = 2^256 - 1
```

Chưa overflow.

Nhưng nếu:

```
balance = 10
amount = 2^256 - 5
```

thì:

```
10 + (2^256 - 5)
= 5 (overflow)
```

→ 5 < amount\
→ **trigger FlagRevealed**

Ta đi deposit nhỏ trước

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

balance = 10

tiếp theo ta overflow nó (số to to là 2^256-5)

lúc đo, 10 + (2^256 - 5)\
\= 5 (overflow)

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

nhìn sang bên dịch vụ web ta được flag

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
