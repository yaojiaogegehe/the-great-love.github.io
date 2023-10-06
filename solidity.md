# solidity

#### 代码基础结构

```solidity
// SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract HelloWorld{
    function greet() public pure returns(string memory){
        return "hello world";
    }
}
```

其中第一行是版权声明，每个solidity智能合约都要写。

第二行是版本声明，用来表示编译器按照哪个版本来编译智能合约。

```solidity
pragma solidity >=0.8.0 <0.9.0 // 表示solidity版本在0.8.0及以上，但在0.9.0以下
pragma solidity >0.8.0 // 表示solidity版本在0.8.0以上，没有任何上限
pragma solidity =0.8.0 // 表示solidity版本智能是0.8.0
pragma solidity 0.8.0 // 表示solidity版本智能是0.8.0
pragma solidity ^0.8.0 // 表示solidity版本在0.8.0及以上，但不包括0.9.0及以上的版本
pragma solidity ~0.8.0 // 与pragma solidity ^0.8.0 相同
```

第三行是合约定义，contract后面的内容是合约的名称，名称可以是任意字符串，但必须以字母或下划线_开头。



#### 注释语句

通常以//为开头进行注释，其注释有以下好处：

1、用于解释代码含义；

2、注释语句不会被执行。

当然注释也分三种，分别是单行注释，多行注释以及NATSpec格式。

以双斜杠为开头的就是单行注释，只对一行有效

```solidity
// SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
```

以斜杠/和星号*包裹的注释表示多行注释，它可以跨越多行

```solidity
contract HelloWorld{
/*
这是一个
多行注释
可以跨越多行
*/
	function greet() public pure returns(string memory){
		return "Hello World";
	}
}
```

NATSpec格式使用三个斜杠///加上标签的注释方法进行单行注释，其中注释标签是以@开头并在后面跟上标签名称和标签值。

```solidity
/// @title 计算器合约
/// @author BinSchool
/// @notice 这是功能描述
/// @dev 这里是实现细节
contract Calculator{
/// @dev 乘法计算
/// @param a 乘数
/// @param b 被乘数
/// @return c 计算结果
	function multiply(unit a,unit b) public pure returns(unit) {
		return a*b;
		}
}
```



### 一、数据类型

##### 1、值类型

整型，布尔型，地址型，字节型，浮点型，枚举型。

##### 2、引用类型

数组，映射，结构体。

（学习变量的声明和赋值）

其中必须要掌握的有：整型，布尔型，地址型，映射，数组



#### A、整型（必须掌握）

分为无符号整型和有符号整型

##### 1、无符号整型

```solidity
// 无符号整型，而且无符号整型取值只能是正整数和零
unit ucount = 16;
```

上面我们定义一个无符号整型变量 ucount，赋值为16.

uint 存储长度为256位，取值范围为0~(2^256)-1

##### 2、有符号整型

```solidity
// 有符号整型，取值可以是正整数，负整数和零
int count = -1;
```

int 存储长度为256位，取值范围是 -2^255~(2^255)-1

其中用于整型变量的运算符主要是算术运算符，如下：

加+，减-，乘*，除/，取余%，其中取余的除法运算只取结果的整数部分，不进行四舍五入。

```
// int a = 2,int b=3
int a_add_b = a + b; // a_add_b = 5
int a_sub_b = a - b; // a_sub_b = -1
int a_mul_b = a * b; // a_mul_b = 6
int a_div_b = a / b; // a_mod_b = 2
```

除了unit和int之外，还有特定长度的整型，无符号整型有：uint8,uint16,uint24,uint32一直到uint256

有符号整型有：int8,int16,int24,int32一直到int256.

##### 3、极限值

极限值，获取某种整型的最大值和最小值，可以使用type函数，例如我们要获取unit8类型的最大值和最小值，可以用以下代码来实现。

```solidity
// SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract ValueTypes{
	int public max = type(unit8).max;
	int public min = type(unit8).min;
}
```

##### 4、默认值

默认值，在智能合约中声明的整型变量，如果没有赋给初始值，那么它的值默认是0，如下：

```solidity
// SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract ValueTypes{
	int public value_int; // value_int = 0
	int8 public value_int8; // value_int8 = 0
	unit public value_unit; // value_unit = 0
	unit16 public value_unit16; // value_unit = 0
}
```

下面是一个关于整型的完整代码展现：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
contract ValueType{
	uint a = 3;
	int b = -1;
	int public c = int(a) - b;     //因为a是无符号整型，不能直接和有符号整型相减，要先在外面套一个有符号整型将其转换成有符号整型
}
```



#### B、布尔型（必须掌握）

布尔型通常出现在条件语句或循环语句中；是判断条件是否成立，控制执行流程；用bool表示，取值只有两种，true和false。

```solidity
// 布尔类型
bool condition = true;
```

逻辑运算符，用于布尔型变量的运算符。，主要包括三种，逻辑非！；逻辑与&&；逻辑或||

##### 1、非

下面是逻辑非的应用，效果等同于"not"：

```
// bool a = true
bool b = !a; // b = false

// bool a = false
bool b = !a; // b = true
```

##### 2、与

下面是逻辑与的应用，逻辑与&&的效果等同于"and"：

```solidity
// bool a = true, bool b = true
bool c = a && b; // c = true 

// bool a = true, bool b = false
bool c = a && b; // c = false
```

##### 3、或

下面是逻辑或的应用，逻辑或||的效果等同于"or"：

```solidity
// bool a = true, bool b = true 
bool c = a || b; // c = true

// bool a = true, bool b = false
bool c = a || b; // c = true

// bool a = false, bool b = false
bool c = a || b; // c = false
```

##### 4、默认值

和整型类似的，布尔型也有默认值，当合约中声明的布尔型变量，没有给初始值，则默认为false

下面是一个关于布尔型的完整代码展现：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract TF{
    bool a = true;
    bool b = false;
    bool public c = !a;
    bool public d = a && b;
    bool public e = a || b;
}
```



#### C、地址型（必须掌握）

1、用来存储以太坊中的账户地址；

2、地址型变量的储存长度为160位，也就是20个字节；

3、使用16进制字符串来表示，0x开头。

地址的生成原理如下：将未压缩的公钥作为输入值，使用keccak256哈希算法生成一个256位的哈希值，然后截取256位哈希值右边的160位作为账户地址。最后为了便于显示，账户地址使用十六进制字符串来表示，这个字符串的长度为40，并以前缀0x开头。

地址可以进行比较运算，可以使用的操作符有：==、!=、<=、<、>=、>。

例如我们可以用下面的代码来比较两地址是否相等：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
contract AddressCompare{
	address address1 = 0xB2D02Ac73b98DA8baF7B8FD5ACA31430Ec7D4429;
	address address2 = 0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db;
	
	function compareAddresses()public view returns(bool){
		return(address1 == address2);
	}
}
```

地址具有.balance属性，用于返回该账户中以太坊的余额，这是地址最常见的用法，如：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract AddressBalance{
	address account = 0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db;
	
	function getBalance()public view returns(uint){
		return account.balance;
	}
}
```

另外，地址也经常用于转账，我们可以使用地址的.transfer()和.send()方法来进行转账。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract AddressTransfer{
	address account = 0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db;
	
	function transferETH()public{
		payable(account).transfer(1 ether);
	}
}
```



#### D、浮点型（了解即可，无需深入研究）

分为有符号定长浮点数（fixed）和无符号定长浮点数（ufixed）。

我们可以在合约中声明浮点型`常量`，并给他赋值，具体如下：

```solidity
//浮点型常量
fixed constant pi = 3.14159265;
```

不能给浮点型`变量`赋值，具体如下：

```solidity
//浮点型变量
fixed a = 3.14；
```

wei是以太币基本计量单位，也是默认计量单位。

一个以太币（ETH）=10的18次方wei。

在solidity智能合约中对某地址进行一笔转账，可以用如下代码表示：

```solidity
payable(address).transfer(100);
```

上面的语句就是向地址address转账了100wei，而不是100以太币（ETH），因为wei如前面所说是solidity中默认计量单位。

以太币（ETH）在solidity中通常记为ether，一个以太币（ETH）就是1ether。

因为有wei这个单位，所以可以用整数来代替一些简易的浮点数，如0.001ether我们可以用整数10的15次方wei来表示。

当然使用整数代替浮点数有以下好处：

1、避免精度丢失；

由于浮点数在计算机中的表示是有限的，因此使用浮点数来处理货币单位可能会导致精度丢失。这意味着在对浮点数进行运算时，可能会出现微小的舍入误差，这可能会影响智能合约的正确性和安全性。

使用整数可以避免这种问题，因为整数类型在计算机中的表示是精确的。

2、提高运算效率；

计算机硬件上执行整数运算比浮点运算更快。由于以太坊网络需要高效地处理大量的数字交易，因此，使用整数可以提高智能合约的执行效率和响应速度。

3、提升安全性。

使用整数可以避免一些常见的浮点数漏洞，例如浮点数溢出和浮点数除以零错误等，这些漏洞可能导致智能合约的不安全行为。

所以，在 Solidity 中以 `wei` 作为计量单位，就可以使用整数来代替浮点数进行计算，从而提高智能合约的精度、效率和安全性。

在实际的合约中，Solidity 中的浮点数类型几乎没有使用。所以，我们只要了解这种数据类型就可以了，无需深入研究。



#### E、枚举型（了解）

在 Solidity 中，`枚举型` 是一种用户自定义的数据类型，它需要在使用之前进行定义。

要定义一个 `枚举型`，可以使用关键字 `enum` 。`枚举型` 由一组预定义的常量组成，这些常量在枚举列表中定义。

例如下面我们使用enum创建了两个预定义的常量male和female：

```solidity
//定义枚举类型
enum gender {male，female}
```

这里的 `gender` 是定义的枚举型的名称，其中 male 和 female 组成了枚举列表。换句话说，枚举型 `gender` 具有两个可选的值，分别是 male 和 female。

然后，我们就可以使用已定义的枚举型 `gender` 来声明变量，并给它赋值，具体如下：

```solidity
//使用枚举型
gender a = gender.male;
```

值得注意的是，有以下情况会报错，需要注意：

1、枚举型列表不能为空，至少要包含一个成员，否则编译器会报错。

2、定义枚举型的语句，大括号后面不能跟分号；，如果跟着分号也会报错。

3、枚举型只能在全局空间内声明，不能在函数内声明。通常在状态变量前声明。

枚举型的本质是一种用户自定义的类型，并不是 Solidity 的原生数据类型。

枚举型在编译后就会转换为无符号整数 uint8，所以，在 Solidity 中，枚举值可以转换为整数，它的第一个成员的值默认是 0，后面的值依次递增。 

例如：

```solidity
enum status {normal, deleted}
uint8 a = uint8(status.normal) // a = 0
uint8 b = uint8(status.deleted) // b = 1
```

枚举型主要作用是提高代码的可读性，在solidity中并不常用，而且可以用常量来代替，我们了解一下就行。

下面的范例中，包含了对枚举型变量的一些常用操作，以及类型之间的转换。

```solidity
// SPDX-License-Identifier: MIT 
pragma solidity ^0.8.0; 

contract EnumOps {
    // 定义枚举型
    enum gender {male, female}

    // 使用枚举型声明状态变量
    gender private myGender = gender.female;

    // 函数内使用枚举类型
    function useEnum() public returns(gender) {
        gender t = myGender;
        myGender =  gender.male;
        return t; 
    }
   // 枚举型用作返回值
    function returnEnum() public pure returns(gender) {
        return gender.female; 
    }
    // 枚举值转换为整型
    function convertInt() public pure returns(uint) {
        return uint(gender.female); 
    }
    // 整型转换为枚举型
    function convertEnum() public pure returns(gender) {
        return gender(1); 
    }
}
```



### 二、solidity变量

#### A、变量

变量指可以保存数据的内部储存单元，里面的数据可以在程序运行时引用或修改。

变量都有一个名字，称为变量名。由字母、数字或下划线“_”组成的字符串，但不能包含空格或其他特殊字符，也不能以数字开头。

solidity中变量对大小写敏感，所以`symbol`和`Symbol`是两个不同的变量。

solidity中变量没有空值的概念，哪怕你不自定义变量的值它也有默认值。

变量分三种类型：状态变量；局部变量；全局变量。

##### 1、状态变量

指在智能合约中声明的持久化存储变量。也就是说状态变量如果要变动是要记录在区块链上的，永久储存。

状态变量在合约中的不同函数之间共享，可以通过调用函数来读取或修改它的数据值。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
contract StatusVar{
	uint256 MyStatus = 1; //声明状态变量
	
	function GetStatus() public view returns(uint256){
		return MyStatus; //调用状态变量
	}
}
```

##### 2、局部变量

指在函数内部声明的变量，其作用域仅限于该函数内部。

局部变量生命周期仅限于该函数执行过程中，执行完毕后局部变量便会销毁，其设定的值也不可用。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract LocalVar{
	function sum()public view returns(uint256){
		uint a = 1;//声明局部变量a
		uint b = 2;//声明局部变量b
		uint c = a + b;//声明局部变量c
		return c;//使用局部变量c作为返回值
	}
}
```

##### 3、全局变量

指在合约顶层预先定义的特殊变量，用于提供有关区块链和交易属性的信息。

全局变量由solidity语言提供，用户无权定义或修改，可以在任何位置使用。

比如常见的全局变量有：当前区块的时间戳block.timestamp、区块高度block.number、函数调用者地址msg.sender。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract GlobalVar{
//返回当前函数调用所在的区块号、时间戳、调用者地址
	function getGlobalVars()public view returns(uint,uint,address){
		return (block.number,block.timestamp,msg.sender);
	}
}
```

下面是常见的全局变量：

| 名称                                          | 返回                                                     |
| :-------------------------------------------- | :------------------------------------------------------- |
| blockhash(uint blockNumber) returns (bytes32) | 指定区块的哈希值                                         |
| block.coinbase (address payable)              | 当前区块的矿工地址                                       |
| block.difficulty (uint)                       | 当前区块的难度值                                         |
| block.gaslimit (uint)                         | 当前区块的gaslimit                                       |
| block.number (uint)                           | 当前区块的区块号                                         |
| block.timestamp (uint)                        | 当前区块的时间戳                                         |
| gasleft() returns (uint256)                   | 当前剩余的gas                                            |
| msg.data (bytes calldata)                     | 完成 calldata                                            |
| msg.sender (address payable)                  | 消息发送者的地址，也就是函数调用者的地址                 |
| msg.sig (bytes4)                              | 消息的函数签名，也就是calldata的前四个字节               |
| msg.value (uint)                              | 消息携带的token数量，也就是函数调用时，同时存入的token量 |
| tx.gasprice (uint)                            | 交易的gas价格                                            |
| tx.origin (address payable)                   | 交易的发送方地址                                         |

以上就是变量的三种类型，另外下面是变量命名需要注意的问题：

不应该用solidity保留的关键词作为变量名。例如int、break或return等变量名无效；

不应该以数字开头，必须以字母或下划线作为变量名。例如`123est`是个无效的变量名，但`_123est`有效；

变量名是区分大小的。例如name和Name是两个不同的变量名。



#### B、可见性

状态变量一个非常重要的属性就是可见性。

可见性是用来控制状态变量在合约内部还是外部的访问权限。

solidity为状态变量提供3种可见性修饰符，分别是`public`，`private`,`internal`，用于限制状态变量的可见性。

##### 1、public

如果可见性为public，也就是公开变量，那么在合约内部或外部都可以访问这个状态。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract PublicVisivility{
	uint public delta = 8;//可见性声明为public
	
	function addDelta(uint num)public view returns(uint){
		uint sum = num + delta;//函数内可以使用状态变量delta
		return sum;
	}
}
```

##### 2、private

如果状态变量的可见性声明为private，也就是私有变量，那就只能在合约内部访问这个状态变量，而不能从外部访问。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract PrivateVisibility{
	uint private delta = 8;//可见性声明为private
	
	function addDelta(uint num)public view returns(uint){
		uint sum = num + delta;
		return sum;
	}
}
```

##### 3、internal

internal为内部变量，可以在当前合约和它的继承合约内部访问这个状态变量，而不能从外部访问。

internal和private相比，唯一的区别就是多了一个变量可以在派生合约中使用的特性。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract InternalVisibility{
	uint internal delta = 1;//可见性声明为internal
	
	function addDelta(uint num)public view returns(uint){
		uint sum = num + delta;
   		return sum;
	}
}
//InheritedVisibility就是InternalVisibility的继承合约
contract InheritedVisibility is InternalVisibility{
	function getDelta()public view returns(uint){
		return delta;//派生合约依旧可以调用母合约的状态变量
	}
}
```

##### 4、默认可见值

状态变量在声明时，没有指定可见性，那么它可见性就为默认的`internal`。

也就是说

```solidity
uint a = 1;
```

等价于

```solidity
uint internal a = 1;
```

##### 5、可见性的用处

a、访问控制

通过在状态变量上设置适当可见性，可以实现对合约功能的访问控制。例如将敏感数据储存在私有状态变量中，只允许合约内部特定函数访问，从而确保外部只有通过授权才可以访问这些数据。

b、封装数据

通过将状态变量声明为private，可以限制对其数据的直接访问，只能通过合约内部函数来访问，这样可以有效封装内部数据，避免外部访问者直接操作状态变量，确保数据的安全性和一致性。

当然，除了状态变量外还有函数也有可见性的限制，后面再更新。



#### C、默认值

solidity智能合约中所有的状态变量和局部变量都有默认值。

##### 1、基本类型的默认值

下面我们做一个整理，将所有数据类型的默认值都整理出来。

`bool`的默认值为false；

`int`的默认值为0，包括各种长度的有符号整型，如int8，int16，……；

`uint`的默认值为0，包括各种长度的无符号整型，如uint8，uint16……；

`address`的默认值为：0x0000...，共40个0；

`bytes32`的默认值为：0x0000...，共64个0；

`string`的默认值为："";

`enum`的默认值是它列表中的第一项。

我们可以通过下列代码来更好的记住这些类型的默认值：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract VarDefault{
	enum status{normal,deleted}//定义枚举类型
	
	bool public a;//false
	int public b;//0
	uint public c;//0
	address public d;//0x00..共40个0
	bytes32 public e;//0x00..共64个0
	string public f;//""
	status public g;//0,也就是status.normal
}
```

##### 2、符合类型的默认值

solidity中有些类型是复合类型，也就是由多个基本类型组合而成，比如结构体`struct`。

```solidity
struct book{
	string name;//设置book中name类型为string
	string author;
	uint price;//设置book中price类型为uint
}
```

复合类型的默认值是由每一个数据项的默认值组成。

我们可以使用上面的符合类型`book`去声明一个变量`mybook`

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract VarDefault{
	struct book{
		string name;
		string author;
		uint price;
	}
	book public mybook;//默认值为{"","",0}
}
```

##### 3、delete命令

`delete`并不是删除一个变量，而是对变量重新初始化，使其为默认值。具体展现功能的代码如下：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract Delete{
	int public a = 1;//声明一个public的int变量的a为1
	function deletea()public{//定义一个公共函数deletea
		delete a;//定义函数的功能
	}
}
```



#### D、常量

solidity智能合约中一个状态变量值恒定不变，就可以使用关键词`constant`或`immutable`进行修饰，把其定义为常量。

下面是常量和变量语句的区别：

```solidity
string constant SYMBOL = "WETH";//这是常量

uint immutable TOTAL_SUPPLY = 1000;//这是常量

uint a = 1;//这是变量
```

常量命名规则与变量相同，但用大写字母表示，单词间用_连接，这样与变量形成区分。

状态变量一旦声明为constant和immutable后就不能更改它的值了。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract changshu{
	string constant A = 'SB';
	int b = 1;
	int constant C = 1;
	function get1()public pure returns(string memory){//因为要返回的A常量返回值类型是string memory，所以要在括号里明确指定
		return A;
	}
	function get2()public view returns(int){
		int sum = b + C;
		return sum;
	}
}
```

##### 1、constant和iimmutable的区别

两者的区别表现在下面两方面：

①初始化时机

`constant`关键词修饰的状态变量必须在声明时就立即显式赋值，然后不再允许修改。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract ConstType{
	uint public constant A = 18;
}
```

而`immutable`除了具有`constant`立即显示赋值的特性外，也可以在合约部署时（也就是构造函数constructor）赋值。但一旦赋值后就不能修改。

具体代码实现可如下：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyContract {
	uint immutable public A = 1;//这里起到和上面constant相同的声明即赋值的特性
	uint immutable public B;

    constructor() {
        B = block.timestamp;
    }
}
```

②适用的数据类型

`constant`可以修饰任何数据类型。

`immutable`只能修饰值类型，如：int、uint、bool、address等，不能修饰string、bytes等引用类型。

##### 2、使用常量的好处

①代码可读性

②代码重用

③预防错误（可以预防错误的值更改和错误赋值操作）

④节省Gas费



### 三、solidity函数

#### A、函数

##### 1、函数定义

关键词`function`用来定义一个函数，后面跟着函数名、参数列表、可见性（visibility）、状态可变性（statemutability）以及返回值。

下面我们直接解析一个加法函数：

```solidity
function add(uint a,uint b)public pure returns(uint){
	return a + b;
}
```

这个函数名为add，它需要两个整型参数a和b，函数可见性为public，状态可变性是pure，最后返回一个整型值。

##### 2、命名规则

函数命名规则与变量相同，不能包含空格或其他特殊字符，也不能以数字开头。

通常开头小写，后面采用驼峰形式。

##### 3、可见性

函数可见性规则与状态变量基本相同，但比状态变量多了一种`external`。

函数可见性共四种，分别是`private`、`public`、`internal`、`external`，用于限制函数的使用范围。

```
private
修饰的函数只能在所属智能合约的内部调用

public
修饰的函数可以从任何地方调用，它既可以在智能合约内部调用，也可以从合约外部调用

internal
修饰的函数可以在所属的智能合约内部调用，也可以在继承合约中调用

external
修饰的函数只能在合约外部调用，不能在合约内部调用，如果一定要在外部调用一定要在函数前面加上'this.'

综上，如果智能合约某个函数要提供给外部使用，那么其可见性必须为external或者public
```

##### 4、返回值

在solidity中，函数可以没有返回值，也可以返回一个或者多个值。返回值可以是任何有效的数据类型。

在函数声明中，需要使用returns关键字指定返回值的类型。

在函数中可以有两种方式来返回结果值：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract ReturnValue{
	function add1(uint a,uint b)public pure returns(uint){
		return a + b;
	}


	function add2(uint a,uint b)public pure returns(uint sum){//这里的返回值名称可以自己定义，我定义为sum
		sum = a + b;
	}
}
```

当然在solidity中函数可以有多个返回值，使用return返回结果时，需要使用括号包裹所有返回值。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract DuoGeFHZ{
	function addDuoValue(uint a,uint b)public pure returns(uint,uint){
	uint sum1 = a + b;
	uint sum2 = a * b;
	return (sum1,sum2);
	}
}
```

##### 5、函数调用

函数的调用只要使用函数名，并传入相应的参数即可。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract FunctionTest{
	function add(uint a,uint b)public pure returns(uint){
		return a + b;
	}
	
	function mul(uint a,uint b)public pure returns(uint){
		return a * b;
	}
	
	function addAndMul(uint a,uint b)public pure returns(uint,uint){
		uint value1 = add(a,b);//调用函数add
		uint value2 = mul(a,b);//调用函数mul
		return(value1,value2);
	}
}
```



#### B、状态可变性

















# 你要明白的地方（别搞混了）

1、view和pure的区别。

```
1、view修饰的函数：
可以读取但不能修改状态变量
可以进行外部函数调用
不会消耗gas

2、pure修饰的函数:
不能读取或者修改状态变量
不能进行外部函数调用
不会消耗gas
```

