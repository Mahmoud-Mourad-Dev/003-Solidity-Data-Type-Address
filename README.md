# Solidity-Data-Type-Address
## Address Holds a 20-byte Ethereum address, 0xe688b84b23f322a994A53dbF8E15FA82CDB71127
it might be a smart contract that was not built to accept Ether.
## Address payable Cand send and recieve eth (transfer,send)
```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.25;
contract addressTest{
    address public myAddress = 0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2;
    address payable  public myPayableAddress = payable(0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2);


}
```
## Type Conversion
### In Solidity, type conversion refers to converting one data type to another when required
### Implicit conversions from address payable to address are allowed, whereas conversions from address to address payable must be explicit via payable(<address>).


## الفرق بين impilcit و expilcit
في البرمجة، التحويل الضمني (Implicit Conversion) و التحويل الصريح (Explicit Conversion) هما طريقتان لتحويل نوع البيانات من نوع إلى آخر. الاختلاف الرئيسي بينهما يعتمد على ما إذا كان التحويل يتم تلقائيًا بواسطة المترجم أو يتطلب تدخلًا مباشرًا من المبرمج.

 التحويل الضمني (Implicit Conversion)
التحويل الضمني يحدث تلقائيًا من قبل المترجم عندما يكون التحويل آمنًا، أي عندما لا يؤدي التحويل إلى فقدان البيانات أو مشاكل منطقية. يتم التحويل دون الحاجة إلى أن يقوم المبرمج بكتابة أي كود إضافي لتحويل النوع.

 التحويل الصريح (Explicit Conversion)
التحويل الصريح يتطلب تدخل المبرمج للإشارة بوضوح إلى أن التحويل مطلوب. يُستخدم هذا النوع من التحويل في الحالات التي قد تكون خطيرة أو قد تؤدي إلى فقدان بيانات، وبالتالي لا يتم إجراؤها تلقائيًا.

## Implicit conversions from address payable to address are allowed, whereas conversions from address to address payable must be explicit via payable(<address>).

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
## In Solidity, explicit conversions are allowed between the address type and several other types, including uint160, integer literals, bytes20, and contract types. This flexibility allows developers to convert between these types when needed, but it must be done explicitly due to the potential for unsafe operations or data loss
1. Conversion between address and uint160:
An Ethereum address is a 160-bit (20-byte) value, which can be represented by the uint160 type.
Conversion between address and uint160 is allowed, but must be done explicitly to avoid unintended results.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.25;
contract addressTest{
    
address addr = 0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2;
uint160 num = uint160(addr);  // Explicit conversion to uint160
address addrFromUint = address(num);  // Explicit conversion back to address

}
```
2. Conversion from Integer Literals to address:
Integer literals can be explicitly converted to an address type. This can be useful in tests or if you know the numeric representation of an address.
However, this should be used with caution since incorrect conversion can result in invalid or unintended addresses.

## I try to convert from integers 256 to address i get error

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






