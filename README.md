# Solidity-Data-Type-Address
  1-Address Holds a 20-byte Ethereum address,it might be a smart contract that was not built to accept Ether.
  2-Address payable Can send and recieve eth (transfer,send)
  
```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.25;
contract addressTest{
    address public myAddress = 0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2;
    address payable  public myPayableAddress = payable(0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2);


}
```
## Type Conversion
In Solidity, type conversion refers to converting one data type to another when required Implicit conversions from address payable to address are allowed, whereas conversions from address to address payable must be explicit via payable(<address>).


## الفرق بين impilcit و expilcit
في البرمجة، التحويل الضمني (Implicit Conversion) و التحويل الصريح (Explicit Conversion) هما طريقتان لتحويل نوع البيانات من نوع إلى آخر. الاختلاف الرئيسي بينهما يعتمد على ما إذا كان التحويل يتم تلقائيًا بواسطة المترجم أو يتطلب تدخلًا مباشرًا من المبرمج.

 التحويل الضمني (Implicit Conversion)
التحويل الضمني يحدث تلقائيًا من قبل المترجم عندما يكون التحويل آمنًا، أي عندما لا يؤدي التحويل إلى فقدان البيانات أو مشاكل منطقية. يتم التحويل دون الحاجة إلى أن يقوم المبرمج بكتابة أي كود إضافي لتحويل النوع.

 التحويل الصريح (Explicit Conversion)
التحويل الصريح يتطلب تدخل المبرمج للإشارة بوضوح إلى أن التحويل مطلوب. يُستخدم هذا النوع من التحويل في الحالات التي قد تكون خطيرة أو قد تؤدي إلى فقدان بيانات، وبالتالي لا يتم إجراؤها تلقائيًا.

 Implicit conversions from address payable to address are allowed, whereas conversions from address to address payable must be explicit via payable(<address>).

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.25;
contract addressTest{
    
address payable payableAddr = payable(0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2);
address addr = payableAddr;  // تحويل ضمني
}
```

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.25;
contract addressTest{
    
address addr = 0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2;
address payable payableAddr = payable(addr);  // تحويل صريح
}
```
In Solidity, explicit conversions are allowed between the address type and several other types, including uint160, integer literals, bytes20, and contract types. This flexibility allows developers to convert between these types when needed, but it must be done explicitly due to the potential for unsafe operations or data loss Conversion between address and uint160:An Ethereum address is a 160-bit (20-byte) value, which can be represented by the uint160 type.Conversion between address and uint160 is allowed, but must be done explicitly to avoid unintended results.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.25;
contract addressTest{
    
address addr = 0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2;
uint160 num = uint160(addr);  // Explicit conversion to uint160
address addrFromUint = address(num);  // Explicit conversion back to address

}
```
Conversion from Integer Literals to address:Integer literals can be explicitly converted to an address type. This can be useful in tests or if you know the numeric representation of an address.
However, this should be used with caution since incorrect conversion can result in invalid or unintended addresses.I try to convert from integers 256 to address i get error

```solidity
pragma solidity ^0.8.0;
contract IntegerToAddressConversion {
    function convertToAddress(uint256 num) public pure returns (address) {
        address addr = address(num);
        return addr;
    }
}
/* ERORR
TypeError: Explicit type conversion not allowed from "uint256" to "address".
 --> address.sol:6:24:
  |
6 |         address addr = address(num);
  |                        ^^^^^^^^^^^^
*/
```

in Solidity, direct type conversion from uint256 to address is not allowed due to security reasons. To convert a uint256 value to an address, you need to use a different approach that involves bitwise operations or the address(uint256) typecast.Here's an updated example using bitwise operations to convert a uint256 to an address in Solidity:

```solidity
pragma solidity ^0.8.0;
contract IntegerToAddressConversion {
    function convertToAddress(uint256 num) public pure returns (address) {
        address addr = address(uint160(num));
        return addr;
    }
}
```
In Solidity, explicit conversion between the address type and the bytes20 type is allowed. Since both types are 20 bytes in size (160 bits), you can convert an address to bytes20 and vice versa using explicit type casting
```solidity

// SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
pragma solidity ^0.8.0;

contract AddressToBytes20 {
    function convertAddressToBytes20(address addr) public pure returns (bytes20) {
        // Explicit conversion from address to bytes20
        return bytes20(addr);
    }
}

```
```solidity
pragma solidity ^0.8.0;

contract Bytes20ToAddress {
    function convertBytes20ToAddress(bytes20 b) public pure returns (address) {
        // Explicit conversion from bytes20 to address
        return address(b);
    }
}
```
In Solidity, the address payable type represents an Ethereum address that can receive Ether, as opposed to a regular address which cannot send or receive Ether directly. The conversion between address or contract types to address payable must be done explicitly using the payable(...) conversion, but there are specific rules governing when this conversion is allowed.
Conversion from Contract Type to address payable:
A contract can be converted to address payable only if it can receive Ether. This means the contract must have a receive() function or a payable fallback function.
If the contract does not have one of these functions, the conversion to address payable will not be allowed.

```solidity


// SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
contract PayableContract {
    receive() external payable {}  // This contract can receive Ether
}

contract Example {
    function convertContractToAddressPayable(PayableContract myContract) public pure returns (address payable) {
        return payable(myContract);  // Valid because the contract has a receive() function
    }
}
// payable(0) is valid and is an exception to this rule.

```

f you convert a type that uses a larger byte size to an address, for example bytes32, then the address is truncated. To reduce conversion ambiguity, starting with version 0.4.24, the compiler will force you to make the truncation explicit in the conversion. Take for example the 32-byte value 0x111122223333444455556666777788889999AAAABBBBCCCCDDDDEEEEFFFFCCCC.
By forcing explicit conversions, Solidity ensures that developers are fully aware of the truncation that occurs when converting larger byte-sized values to an address.
```solidity

// SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
contract bytes32Tobytes20 {
    bytes32 public b = 0x111122223333444455556666777788889999AAAABBBBCCCCDDDDEEEEFFFFCCCC;
    address public addr1 = address(uint160(uint256((b))));
    address public addr2 = address(uint160(uint256((b))));
// address(uint160(bytes20(b))): Uses the first 20 bytes of the bytes32 value.
// address(uint160(uint256(b))): Uses the last 20 bytes of the bytes32 value.
}
```










