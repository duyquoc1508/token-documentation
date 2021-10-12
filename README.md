# Token documentations

- [1 Giới thiệu](#1-tổng-quan)
- [2 Token](#2-token)
  - [2.1 Phương thức](#21-phương-thức)
    - [2.1.1 name](#211-name)
    - [2.1.2 symbol](#212-symbol)
    - [2.1.3 decimals](#213-decimals)
    - [2.1.4 totalSupply](#214-totalsupply)
    - [2.1.5 balanceOf](#215-balanceof)
    - [2.1.6 getOwner](#216-getowner)
    - [2.1.7 transfer](#217-transfer)
    - [2.1.8 transferFrom](#218-transferfrom)
    - [2.1.9 approve](#219-approve)
    - [2.1.10 allowance](#2110-allowance)
    - [2.1.11 increaseAllowance](#2111-increaseallowance)
    - [2.1.12 decreaseAllowance](#2112-decreaseallowance)
    - [2.1.13 burn](#2113-burn)
    - [2.1.14 burnFrom](#2114-burnfrom)
  - [2.2 Sự kiện](#22-sự-kiện)
    - [2.2.1 Transfer](#221-transfer)
    - [2.2.2 Approval](#222-approval)
- [3 Tương tác với hợp đồng thông minh](#3-tương-tác-với-hợp-đồng-thông-minh)

## 1 Tổng quan

## 2 Token

### 2.1 Phương thức

#### 2.1.1 name

```
name() -> uint256
```

- Trả về tên của token. Ví dụ: "My Token".

#### 2.1.2 symbol

```
symbol() -> string
```

- Trả về kí hiệu (tên viết tắt) của token. Ví dụ: “MTK”.

#### 2.1.3 decimals

```
decimals() -> uint8
```
- Trả về số chữ số thập phân được sử dụng. Ví dụ: decimals = 2 và sô dư là 107 thì số lượng token nên hiển thị cho user là 1.07 (107 / 10 \*\* 2).
- Các token thường được chọn giá trị là 18 giống như Ethereum.
- Lưu ý về `decimals`: 
  - Solidity và EVM không hỗ trợ tính toán trên các sô thập phân (dấu chấm động), chỉ có thể tính toán trên các số nguyên. Điều này dẫn đến vấn đề là không thể chuyển 1 lượng lẻ (ex: 1.5) token từ tài khoản này sang tài khoản khác. Để giải quyết vấn đề này ERC20 cung cấp thêm 1 trường dữ liệu là `decimals` để chỉ định token có bao nhiêu vị trí thập phân. Để chuyển 1.5 token, `decimals` ít nhất phải là 1 vì số đó có 1 chữ số thập phân.
  - Giá trị này chỉ sử dụng cho mục đích hiển thị, không ảnh hưởng đến logic của smart contract và các ví, sàn giao dịch,... phải điều chỉnh giá trị hiển thị theo `decimals`
  - Vì vậy, nếu bạn muốn chuyển 5 token bằng cách sử dụng smart contract với decimals bằng 18, tham số truyền vào hàm sẽ tương tự như sau:
  ```
  transfer(recipient, 5 * 10 ^ 18);
  ```

#### 2.1.4 totalSupply

```
totalSupply() -> uint256
```

- Trả về số lượng token tồn tại

#### 2.1.5 balanceOf

```
balanceOf(address account) -> uint256
```

**Parameters**
|Param |Required |Type |Description|
|-------|-----------|-------|-----------|
|account|Yes\*|Address|Địa chỉ của tài khoản cần kiểm tra số dư|

- Trả về số lược token mà tài khoản `account` hiện có.

#### 2.1.6 getOwner

```
getOwner() -> address
```

- Trả về địa chỉ của chủ sở hữu token này.

#### 2.1.7 transfer

```
transfer(address recipient, uint256 amount) -> bool
```

**Parameters**
|Param |Required |Type |Description|
|-------|-----------|-------|-----------|
|recipient|Yes*|String|Địa chỉ người nhận|
|amount|Yes*|String hoặc BN |Số lượng cần chuyển|

- Chuyển 1 lượng (`amount`) token từ địa chỉ người gọi hàm này tới địa chỉ khác (`recipient`).

- Trả về giá trị boolean cho biết thao tác có thành công hay không.

- Phát ra 1 sự kiện `Transfer`

#### 2.1.8 transferFrom

```
transferFrom(address spender, address recipient, uint256 amount) -> bool
```

**Parameters**
|Param |Required |Type |Description|
|-------|-----------|-------|-----------|
|spender|Yes*|String|Địa chỉ người gửi token|
|recipient|Yes*|String|Địa chỉ người nhận token|
|amount|Yes\*|String hoặc BN |Số lượng token cho phép|

- Chuyển 1 lượng (`amount`) token từ địa chỉ này (`spender`) tới địa chỉ khác (`recipient`). Người gọi chức năng sử dụng token mà chủ sở hữu cho phép, `amount` sẽ khấu trừ vào khoản `allowance` của tài khoản gọi chức năng này.
- Chức năng này được sử dụng trong trường hợp rút tiền, cho phép contract chuyển token thay cho chủ sở hữu.
- Trả về giá trị boolean cho biết thao tác có thành công hay không.
- Phát ra 1 sự kiện `Transfer`.

#### 2.1.9 approve

```
approve(address spender, uint256 amount) -> bool
```

**Parameters**
|Param |Required |Type |Description|
|-------|-----------|-------|-----------|
|spender|Yes*|String|Địa chỉ người cho phép sử dụng token|
|amount|Yes*|String hoặc BN |Số lượng token cho phép|

- Cho phép (`spender`) dùng 1 lượng (`amount`) token thay cho chủ sở hữu (tài khoản gọi chức năng này). Nếu chức năng này đươc gọi lại, nó sẽ ghi đè lên giá trị cho phép hiện tại.
- Trả về giá trị boolean cho biết thao tác có thành công hay không.
- Phát ra 1 sự kiện `Approval`

#### 2.1.10 allowance

```
allowance(address owner, address spender) -> uint256
```

**Parameters**
|Param |Required |Type |Description|
|-------|-----------|-------|-----------|
|owner|Yes*|String|Địa chỉ tài khoản sở hữu token|
|spender|Yes*|String |Địa chỉ tài khoản cho phép sử dụng token|

- Trả về lượng token còn lại mà tải khoản (`spender`) sẽ được phép sử dụng thay mặc `owner` qua phương thức `transferFrom`. Mặc định sẽ là giá trị 0.
- Giá trị này thay đổi khi các hàm `approve` hoặc `transferFrom` đươc gọi

#### 2.1.11 increaseAllowance

```
increaseAllowance(address spender, uint156 addedValue) -> bool
```

**Parameters**
|Param |Required |Type |Description|
|-------|-----------|-------|-----------|
|spender|Yes*|String|Địa chỉ tài khoản người được phép sử dụng token thay thế chủ sở hữu|
|addedValue|Yes*|String or BN |Số lượng token cho phép sử dụng cộng thêm|

- Tăng số lượng (`addedValue`) token mà chủ sở hữu (người gọi chức năng này) cho phép người khác (`spender`) sử dụng.

- Phát ra một sự kiện `Approval` cho biết allowance được cập nhật.

#### 2.1.12 decreaseAllowance

```
decreaseAllowance(address spender, uint256 subtractedValue) -> bool
```

**Parameters**
|Param |Required |Type |Description|
|-------|-----------|-------|-----------|
|spender|Yes*|String|Địa chỉ tài khoản người được phép sử dụng token thay thế chủ sở hữu|
|subtractedValue|Yes*|String or BN |Số lượng token bị giảm bớt|

- Giảm một lượng (`subtractedValue`) token mà chủ sở hữu (người gọi chức năng này) cho phép người khác (`spender`) sử dụng.

- Phát ra một sự kiện `Approval` cho biết allowance được cập nhật.

#### 2.1.13 burn

```
burn(uint256 amount)
```

- Hủy 1 lượng `amount` token từ người gọi, làm giảm `totalSupply`.
- Phát ra 1 sự kiện `Transfer`, `to` được đặt là `address(0)`

#### 2.1.14 burnFrom

```
burnFrom(address account, uint256 amount)
```

- Hủy 1 lượng `amount` token từ `account`, trừ `allowance` của người gọi và số lượng token của owner. Làm giảm `totalSupply`. Liên quan đến giá trị `allowance`

### 2.2 Sự kiện

Các sự kiện này được phát ra tự động khi một số chức năng cụ thể được thực thi, dev sẽ lắng nghe các sự kiện này trên blockchain sau đó cập nhật dữ liệu cho phù hợp.

#### 2.2.1 Transfer

```
event Transfer(address from, address to, uint256 value)
```

- Được phát ra khi một lượng `value` token được chuyển từ tài khoản này (`from`) sang tài khoản khác (`to`).

- Lưu ý: `value` có thể bằng 0.

#### 2.2.2 Approval

- Sự kiện này được phát khi 1 tài khoản cho phép tải khoản khác sử dụng token của mình. Hay khi chức năng `approve` được gọi.

```
event Approval(address owner, address spender, uint256 value)
```

- Được phát ra khi `allowance` của `owner` cho `spender` được đặt bởi 1 lệnh gọi tới `approve`. `value` là lượng allowance mới.

## 3 Tương tác với hợp đồng thông minh

Để tương tác với smart contract cần phải có 2 thông tin sau: contract address, ABI.

- Contract address: Địa chỉ triển khai smart contract
- ABI: Cung cấp interface bao gồm tất cả các thông tin về các hàm, sự kiện.

```csharp
using Nethereum.Web3; using Nethereum.Web3.Accounts;
var contractInstance = web3.Eth.GetContract(abi, receipt.ContractAddress);
```

Khi đã khởi tạo hợp đồng, chúng ta có thể sử dụngcác chức năng cụ thể để tương tác với hợp đồng thông minh. Có thể truy xuất bằng cách sử dụng tên của các hàm trong smart contract

```csharp
var transferFunction = contractInstance.GetFunction("transfer");
var balanceFunction = contractInstance.GetFunction("balanceOf");
```

Trước khi thực hiện các chức năng, chúng ta có thể "estimate" chức năng sẽ tốn bao nhiêu gas. Khi tương tác với hợp đồng thông minh cần phải bỏ ra một số chi phí, chi phí này được gọi là gas. Khi chúng ta cung cấp quá nhiều gas, lượng gas chưa được sử dụng hết sẽ được trả lại, nhưng nếu xảy ra lỗi thì quá trình xử lý giao dịch sẽ bị tiêu thụ hết.

Để ước tính chi phí gas trong hàm truyền của chúng ta, chúng ta có thể gọi `EstimateGasAsync` truyền các tham số tương tự.

```csharp
var gasEstimated = await transferFunction.EstimateGasAsync(senderAddress, null, null, newAddress, amountToSend);
```

Bây giờ chúng ta có "gasEstimated", chúng ta có thể sử dụng giá trị đó làm một trong các tham số cho giao dịch.

- Chuyển tiền token từ tài khoản này qua tài khoản khác `transfer()`

```csharp
var receiptFirstAmountSend = await transferFunction.SendTransactionAndWaitForReceiptAsync(senderAddress, gas, null, null, newAddress, amountToSend);
```

- Kiểm tra số lượng token sau khi chuyển. Dùng `balanceOf()`

```csharp
var balanceFirstAmountSend = await balanceFunction.CallAsync<int>(newAddress);
```

