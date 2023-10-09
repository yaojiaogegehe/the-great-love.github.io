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

##### 2、复合类型的默认值

solidity中有些类型是复合类型，也就是由多个基本类型组合而成，比如结构体`struct`。

```solidity
struct book{
	string name;//设置book中name类型为string
	string author;
	uint price;//设置book中price类型为uint
}
```

复合类型的默认值是由每一个数据项的默认值组成。

我们可以使用上面的复合类型`book`去声明一个变量`mybook`

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

##### 1、constant和immutable的区别

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

对于函数是否会修改合约状态，我们可以用状态可变性来修饰。

函数状态可变性有四种：pure、view、payable、未标记状态。

函数声明时必须设置正确的状态可变性，否则无法通过编译。

例如下面的加法函数，它的状态可变性是pure：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract Add{
	function add(uint a,uint b)public pure returns(uint){
		uint c = a + b;
		return c;
	}
}
```

##### 1、pure状态可变性

称为纯函数，指函数不会读取和修改合约的状态。也就是说，pure函数不会读取和修改链上数据。例如：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract Purehanshu{
	function sum()public pure returns(uint){
		uint a = 1;
		uint b = 2;
		return a + b;
	}
}
```

上述合约中sum函数状态可变性设置为pure，因为函数中只使用了局部变量a、b，没有使用任何状态变量。

如果函数中存在着以下语句，则被视为读取了状态数据，就不能使用 `pure` ，否则无法通过编译。

- 读取状态变量
- 访问 `<address>.balance`
- 访问任何区块、交易、msg等全局变量
- 调用了任何不是纯函数的函数
- 使用包含特定操作码的内联汇编

##### 2、view状态可变性

被称为视图函数，指函数会读取合约状态，但不会进行修改。换言之，view会读取链上数据，但不会改变链上数据，例如：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract viewhanshu{
	uint a = 3;
	uint b = 6;
	function add()public view returns(uint){//这个地方如果你换成pure就会报错，因为pure不能读取状态变量
		return a * b;
	}
}
```

上面合约中add函数状态可变性为view，因为它读取了状态变量a和b的值，但并没有修改。

如果函数中存在以下语句，则被视为修改了状态数据，就不能使用可见性view。

- 修改状态变量
- 触发事件
- 创建其它合约
- 使用了自毁函数 `selfdestruct`
- 调用发送以太币
- 调用任何不是 `view` 或 `pure` 的函数
- 使用了底层调用
- 使用包含特定操作码的内联汇编

##### 3、未标记状态可变性

如果一个函数定义中没标记任何状态可变性，也就是既没标pure也没标view，就意味着这个函数要改变状态的。

比如，修改合约的状态变量或向其他合约发送交易等，例如：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract weibiaoji{
	uint a = 1;
	function setA(uint _a)public{
		a = _a;//重新设置了状态变量a的值
	}
	function sum(uint b)public view returns(uint){
		return a + b;
	}
}
```

上面的合约中setA函数修改了a的值，所以不能被标记为view或者pure。

##### 4、payable状态可变性

一个函数状态可变性为payable，就表示它可以接收以太币，这些以太币是由调用者在调用函数时支付的。因为payable函数接收了以太币，所以它是改变了合约状态的。

payable有什么应用场景？比如在一个竞猜游戏的合约中，它的投注函数就要支付一定以太币，因此需要声明为payable。

如果没有指定payable，那么调用这个函数就不能支付以太币，否则会报错。例如：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract BuyPayable{
	//标记函数标记为payable，表示它可以接收以太币
	function stake(uint teamID)public payable{
	//……
	}
}
```

##### 5、状态可变性的作用

```
安全性
通过将函数状态可变性明确定义为view、pure、payable，或者默认的未标记状态可变性，可以在编译时对函数行为进行验证。
这可以帮助开发者避免不经意间修改合约状态或访问未经授权的外部合约，从而减少潜在安全漏洞。

可靠性
通过明确函数的状态可变性，合约的使用者可以更好理解和预测函数的行为。
这有助于避免意外的副作用和错误的结果。像view、pure修饰的函数在调用时不会产生副作用，因此可以安全被其它函数调用，或者并行执行，提高了合约的可靠性。

互操作性
通过标记函数的可变性，可以提供给其它合约和工具有关函数的重要信息。
例如，payable函数可以接收以太币作为支付，使其可与其他合约进行交互，并支持支付功能。
这种明确的标记有助于确保合约之间的互操作性，并促进合约生态系统的发展。
```

##### 6、状态可变性与Gas

为了防止滥用区块链，以太坊规定，对于改变链上的状态的操作，需要支付一定价值的 gas 费。

因为改变了链上的状态，就需要将这些改变同步到区块链的全部节点，达成全网共识，保证数据一致，这个成本是非常高的。

而对于不改变链上状态的操作，只需要在接入节点上完成，无需全网进行数据同步，这个成本就很低，因此无需付费。

正是基于以上原因，调用 `view` 、`pure` 函数，无需支付 gas，而调用非 `view` 、`pure` 函数就需要支付一定的 gas。

所以，我们在设计合约的时候，为了节约成本，减少支付 gas，就应该仔细划分函数的范围，尽量使用 `view` 、`pure` 函数。



#### C、构造函数constructor

构造函数constructor是一个特殊函数，仅在智能合约部署的时候自动执行，不能被手动调用或再次执行。也就是说构造函数只能在智能合约创建的时候执行唯一的一次。

构造函数用来进行状态变量的初始化工作，而且构造函数只能用关键字`constructor`作为函数名。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract FunConstructor{
	uint a;//设置状态变量
	constructor(uint _a){//构造函数
		a = _a;
	}	
}
```

构造函数constructor前面不用添加function关键字，而且该函数没有返回值。

构造函数可以不带参数，也可以带有任意个参数，这些参数用于在合约创建时传递初始值。

如果构造函数带有参数的话，那么在部署的时候就必须填写参数值。

##### 1、可见性和状态可变性

构造函数的可见性不用设置，可以被任何人调用。而状态可变性则不能设置为pure和view。因为构造函数通常用来初始化状态变量，它是会改变合约状态的。

如果我们在部署合约同时向合约内存入一些以太币时，就需要将构造函数的可见性设置为payable。如下：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract FunConstructor{
	uint a;
	constructor(uint _a)payable{
		a = _a;
	}
}
```

##### 典型案例

下面有一个典型案例，存在于很多实际运行的合约中，是使用构造函数来控制操作权限的例子：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract FunConstructor{
	address _owner;//设置合约部署者这个状态变量
	constructor(){
		_owner = msg.sender;//将合约部署者这个数据保存到之前设置的状态变量中
	}
	function operate()public view{//设置一个检测当前账号是否是合约部署者的检测函数
	require(msg.sender == _owner,"错误");
	}
	function owner()public view returns(address){//设置一个获取合约部署者账号的函数
		return _owner;
	}
}
```



#### D、接收函数receive

##### 1、账户类型

以太坊区块链中存在两种类型的账户，为外部账户和合约账户。

```
外部账户：
缩写为EOA，用以储存以太币和执行以太币ETH的交易。账户可以向其他账户发送以太币或接收其他账户发来的以太币。
我们在钱包中管理的账户通常是外部账户，如小狐狸里添加或生成的账户。
外部账户会有个与之相关的以太坊地址，以0x开头，长度20字节的十六进制数，如：0x7CA35...9C6F。
外部账户都有一个与之相对应的私钥，只有持有私钥的人才能对交易进行签名，所以外部账户非常适用于资金管理的身份验证。

合约账户：
缩写为CA，我们在以太坊区块链上部署一个合约后，都会产生一个对应的账户，称为合约账户。
合约账户主要用于托管合约，里面包含着智能合约的二进制代码和状态信息。
合约账户也有一个以太坊地址，在部署智能合约时产生，也是以0x开头的长度为20字节的十六进制数。
但合约账户没有私钥，只能由智能合约中代码逻辑进行控制，它在一定条件下，也可以用来存储以太币ETH。
```

##### 2、Receive函数

在以太坊区块链上部署的智能合约时产生的合约账户，并不是都能存入以太币ETH。

一个智能合约如果允许存入以太币，就必须实现receive或者fallback函数，如果合约中没有定义这两个函数之一，就不能接收以太币。

如果只是为了能让合约账户存入以太币，推荐receive函数，因为简单明了目的明确，而fallback函数用途更复杂一些。

定义receive函数的格式如下：

```solidity
receive()external payable{
	//这里可以添加自定义的处理逻辑，但也可以为空
}
```

receive函数有下面几个特点：

1）无需用function声明；

2）参数可以为空；

3）可见性必须设置为external；

4）状态可变性必须设置为payable。

当外部地址向智能合约地址发送以太币时，将触发执行receive函数。

我们可以在函数体内不写任何自定义的处理逻辑，它仍然能接收以太币，这也是最常见的方式。

如果必须在receive函数中添加处理语句的话，最好不要添加太多业务逻辑。因为外部调用send和transfer方法进行转账时为防止重入攻击，gas会限制在2300左右，如果receive函数太复杂，会很容易耗尽gas从而触发交易回滚。

receive函数中一般会执行一些简单的记录日志的动作，比如触发event。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract FunReceive{
	event Re(msg.sender,uint amount);//定义接收事件
	
	receive() external payable{//接收ETH时，触发先前定义的Re事件
		emit re(msg.sender,msg.value);
	}
}
```



### 四、solidity控制语句

#### A、运算符

Solidity 支持多种运算符，用于对变量和表达式执行数学计算、条件判断、位操作和赋值等操作。

Solidity 支持的运算法包括 6 类：算术运算符、比较运算符、逻辑运算符、位运算符、赋值运算符、条件运算符。

##### 1、算数运算符（常用）

算术运算符用于执行基本的数学运算，对数字进行加法、减法、乘法和除法等操作。

假设A为10，B为20

| 序号 | 运算符与描述                                                 |
| :--- | :----------------------------------------------------------- |
| 1    | **+ (加)**  两数相加，**例如:** A + B = 30                   |
| 2    | **- (减)**  两数相减，**例如:** A - B = -10                  |
| 3    | *** (乘)**  两数相乘，**例如:** A * B = 200                  |
| 4    | **/ (除)**  两数相除，**例如:** B / A = 2                    |
| 5    | **% (取模)**  取余数运算，**例如:** B % A = 0                |
| 6    | **++ (递增)**  递增运算，**例如:** A++ = 11，等同于 A+1 = 11 |
| 7    | **-- (递减)**  递减运算，**例如:** A--= 9，等同于 A-1 = 9    |

下面是一个示范代码：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract suansu{
	int a = 10;
	int b = 20;
	function jia()public view returns(int){
		return a + b;
	}
	function jian()public view returns(int){
		return a - b;
	}
	function cheng()public view returns(int){
		return a * b;
	}
	function chu()public view returns(int){
		return a / b;
	}
	function quchu()public view returns(int){
		return a % b;
	}
	function dizeng()public returns(int){
		a++ ;
		return a;
	}
	function dijian()public returns(int){
		a-- ;
		return a;
	}
}
```

##### 2、比较运算符（常用）

比较运算符用于比较两个值之间的关系，并返回一个布尔值。

假设变量 A 的值为 10，变量 B 的值为 20。

| 序号 | 运算符与描述                                     |
| :--- | :----------------------------------------------- |
| 1    | **== (等于)**  **例如:** A == B，结果为 false    |
| 2    | **!= (不等于)**  **例如:** A != B，结果为 true   |
| 3    | **> (大于)**  **例如:** A > B，结果为 false      |
| 4    | **< (小于)**  **例如:** A < B，结果为 true       |
| 5    | **>= (大于等于)**  **例如:** A > B，结果为 false |
| 6    | **<= (小于等于)**  **例如:** A <= B，结果为 true |

下面是示范代码：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract OperatorCompare {
  int a = 10;
  int b = 20;
  function equal() public view returns(bool){ 
    return a==b; // 结果为 false
  } 
  function notEqual() public view returns(bool){ 
    return a!=b; // 结果为 true
  } 
  function greater() public view returns(bool){ 
    return a>b; // 结果为 false
  }
  function less() public view returns(bool){ 
    return a<b; // 结果为 true
  } 
  function greaterOrEqual() public view returns(bool){ 
    return a>=b;  // 结果为 false
  }
  function lessOrEqual() public view returns(bool){ 
    return a<=b;  // 结果为 true
  } 
}
```

##### 3、逻辑运算符（常用）

逻辑运算符用于在条件表达式组合和操作布尔值。

逻辑运算符经常用于条件语句、循环语句和布尔表达式中，通过逻辑关系决定执行路径，来实现复杂的流程控制。

假设变量 A 的值为 true，变量 B 的值为 false。

| 序号 | 运算符与描述                                                 |
| :--- | :----------------------------------------------------------- |
| 1    | **&& (逻辑与)** 如果两个操作数都为 true，则返回 true；否则返回 false。 **例如:** (A && B) 为 false。 |
| 2    | **\|\| (逻辑或)** 如果至少一个操作数为 true，则返回 true；否则返回 false。 **例如:** (A \|\| B) 为 true。 |
| 3    | **! (逻辑非)** 将布尔值取反，如果操作数为true，则返回false；如果操作数为false，则返回true。 **例如:** !A 为 false。 |

下面是示范代码：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract OperatorLogic {
  bool a = true;
  bool b = false;
  function and() public view returns(bool){ 
    return a&&b; // 结果为 false
  } 
  function or() public view returns(bool){ 
    return a||b; // 结果为 true
  } 
  function not() public view returns(bool){ 
    return !a; // 结果为 false
  }
}
```

##### 4. 位运算符

Solidity 支持多种位运算符，但位运算符在实际的使用场景并不多，我们可以大体了解一下，以后用到再去深入学习。

假设变量 A 的值为 2，变量 B 的值为 3。

| 序号 | 运算符与描述                                                 |
| :--- | :----------------------------------------------------------- |
| 1    | **& (位与)** 对其整数参数的每个位执行位与操作。 **例如:** (A & B) 为 2。 |
| 2    | **\| (位或)** 对其整数参数的每个位执行位或操作。 **例如:** (A \| B) 为 3。 |
| 3    | **^ (位异或)** 对其整数参数的每个位执行位异或操作。 **例如:** (A ^ B) 为 1。 |
| 4    | **~ (位非)** 一元操作符，反转操作数中的所有位。 **例如:** (~B) 为 -4。 |
| 5    | **<< (左移位))** 将第一个操作数中的所有位向左移动，移动的位置数由第二个操作数指定，新的位由0填充。将一个值向左移动一个位置相当于乘以2，移动两个位置相当于乘以4，以此类推。 **例如:** (A << 1) 为 4。 |
| 6    | **>> (右移位)** 左操作数的值向右移动，移动位置数量由右操作数指定。 **例如:** (A >> 1) 为 1。 |

##### 5. 赋值运算符

赋值运算符用于将一个值赋给变量或表达式。我们平时使用最多的就是简单赋值 “=”，其它的赋值运算符仅仅起到简写的作用，都可以使用简单赋值代替。

| 序号 | 运算符与描述                                                 |
| :--- | :----------------------------------------------------------- |
| 1    | **= (简单赋值)**  将右侧操作数的值赋给左侧操作数。  **例如:** C = A + B 表示 A + B 赋给 C。 |
| 2    | **+= (相加赋值)**  将右操作数添加到左操作数并将结果赋给左操作数。  **例如:** C += A 等价于 C = C + A。 |
| 3    | **−= (相减赋值)**  从左操作数减去右操作数并将结果赋给左操作数。  **例如:** C -= A 等价于 C = C – A。 |
| 4    | ***= (相乘赋值)**  将右操作数与左操作数相乘，并将结果赋给左操作数。  **例如:** C *= A 等价于 C = C * A。 |
| 5    | **/= (相除赋值)**  将左操作数与右操作数分开，并将结果分配给左操作数。  **例如:** C /= A 等价于 C = C / A。 |
| 6    | **%= (取模赋值)**  使用两个操作数取模，并将结果赋给左边的操作数。  **例如:** C %= A 等价于 C = C % A。 |

##### 6. 条件运算符

条件运算符是一个三元运算符，用于根据条件的真假选择不同的值。

这是一种简洁的表达式形式，可以由条件语句来代替。

| 序号 | 运算符与描述                                                 |
| :--- | :----------------------------------------------------------- |
| 1    | **? : (条件运算符 )**  **例如：**C?X :Y**，**如果条件 C 为真 ，则取前一个值 X，否则取后一个值 Y。 |

例如：

```
uint value = status==1?100:200;
```

如果 status 的值为 1，那么 value 的值就是 100。

如果 status 的值不为 1，那么 value 的值就等于 200。



#### B、条件语句

Solidity 支持三种条件语句：**if** 语句、**if...else**语句、**if...else if**语句。

##### 1、if语句

语法如下：

```solidity
if (condition){
	//如果条件condition为true，则允许运行这里的代码，否则跳过这段代码
}
```

下面是if语句的示例代码：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract ConditionIf{
	function transfer(uint amount)public pure returns(bool){
	if(amount == 0){
		return false;//如果转账金额amount等于0则返回false
		}
	return true;
	}
}
```

##### 2、if…else语句

语法如下：

```solidity
if (confition){
	//如果条件condition为true，执行这里的代码
}else{
	//如果条件condition为false，就执行这里的代码
}
```

下面是if...else语句的示例代码：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract ConditionIfElse{
	function transfer(uint amount)public pure returns(bool){
		if(amount == 0){
			return false;
		}else{
			return true;
		}
	}
}
```

##### 3、if...else if...语句

if...else if...语法如下：

```solidity
if(condition1){
	//如果condition1为true则执行这里的代码
}else if(condition2){
	//如果condition1为false，condition2为true，就执行这里的代码
}else{
	//以上条件都不满足就执行这里的代码
}
```

下面是if...else if...语句的示例代码：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract ConditionIfElseIf{
	function test(uint status)public pure returns(int){
		if(status == 1){
			return 100;
		}else if(status == 2){
			return 200;
		}else{
			return 0;
		}
	}
}
```



#### C、循环语句

Solidity 提供了循环语句，用于在合约中重复执行特定的代码块。

Solidity 支持 3 种循环语句：**while** 语句、**do...while** 语句、**for** 语句。

另外还提供了两个循环控制语句：**break**、**continue** 语句。

##### 1、while循环

while循环语法如下：

```solidity
while(condition){
	//如果条件condition为true，就循环执行下面的代码
	...
}
```

下面是while语句的示例代码：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract ValueWhile{
	function sum()public view returns(uint){
		uint result = 0;
		uint i = 1;
		while (result <= 10){
			result = result + i;
			i ++ ;
		}
		return result;
	}
}
```

##### 2、do...while循环语句

do...while语句语法如下：

```solidity
do{
	//循环执行以下代码
	...
}while(condition);//如果条件condition为true就继续执行循环
```

下面是do...while语句的示例代码

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract ValueDoWhile{
	function sum()public pure returns(int){
		int result = 0;
		int i = 1;
		do{
			result = result + i;
			i++;
		}while(i <= 10);
		return result;
	}
}
```

##### 3、for循环

for语句语法如下：

```solidity
for(初始化；测试条件；迭代语句){
	//如果测试条件为true，就循环执行下面代码，重复执行直到条件为false为止
	...
}
```

for语句的示例代码如下：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract ValueFor{
	function sum()public pure returns(uint){
		uint result = 0;
		for(uint i = 1;i<=10;i++){
			result = result + i;
		}
		return result;
	}
}
```

##### 4、break语句

break控制语句能推出当前循环语句，并继续执行后面代码。

**在嵌套的多层循环语句中，break只会退出当前循环。**

```solidity
for(uint i = 1;i <= 10;i++){
	result = result + i;
	if (i == 5){
		break;
	}
}
```

##### 5、continue语句

和break语句一样，continue语句也是一种控制语句，用于在循环中跳过当前迭代的**剩余**代码，并继续进行下一次迭代。

```solidity
for(uint i = 1;i <= 10;i++){
	result = result + i;
	if (i == 5){
		continue
	}
}
```



#### D、断言语句

断言语句用于在合约执行过程中进行条件检查和错误处理。

##### 1、require

用于检查函数执行先决条件，也就是说确保满足特定条件后才能继续执行函数。

如果条件不满足的话会中止当前函数的执行，并回滚所有的状态改变。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract AssetRequire{
	function transfer(address A,uint amount)public pure{
		require(A != address(0),"address is zero");
		require(amount > 0,"amount is zero");
	}
}
```

另外，require的功能完全可以用revert来代替，比如下面这句：

```solidity
require(A != address(0),"address is zero");
```

这句完全可以用revert语句来代替，如下：

```solidity
if (A == address(0)){
	revert("address is zero");
}
```

##### 2、asset语句（早期solidity版本遗留，不建议使用）

asset语句行为和require类似，常用于捕捉合约内部编程错误和错误情况。

如果捕捉到异常就会中止当前函数执行，并回滚所有的状态改变。

```solidity
//SPDX-License-Identifier:MIT
pragma solidity^0.8.0;

contract AssertValue{
	function chufa(uint beichushu,uint chushu)public pure returns(uint){
		assert(chushu != 0);//确保除数不为零
		return beichushu/chushu;
	}
}
```

##### 3、require和assert区别

require和assert区别如下：

1. `require` 和 `assert` 参数不同，`require` 可以带有一个说明原因的参数，`assert` 没有这个参数。
2. `require` 通常用于检查外部输入是否满足要求，而 `assert` 用于捕捉内部编程错误和异常情况。
3. `require` 通常位于函数首部来检查参数，`assert` 则通常位于函数内部。
4. `assert` 是 Solidity 早期版本遗留下来的函数，不再建议使用，最好使用 `require` 和 `revert` 代替。



### 五、solidity复合类型

#### A、数组

数组是一个用于储存相同类型元素的数据结构。

##### 1、固定长度数组

在声明时要指定数组长度，并这个长度在编译时就确定了，无法更改。

语法如下：

```solidity
type_name arrayName[length];
```

其中长度必须是大于0的整数。如果要声明一个uint类型的长度为10的数组：book，则如下所示：

```solidity
uint book[10]
```

初始化一个固定长度数组，可以用如下的代码：

```solidity
uint book[3] = [uint(1),2,3];//创建一个book的数组并定义长度和数组内的元素
book[2] = 5;//因为数组从左到右是从0开始数的，第2个元素就是3，这里是将book数组中第三个元素赋值为5
```

而且我们也可以通过索引的方式来访问数组中的元素，如下：

```solidity
uint book3 = book[2];//我们通过索引的方式将数组中第三个元素以book3的变量名来进行访问
```

下面是一个完整的代码：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract GuDinValue{
	uint[3] book = [uint(1),2,3];
	function Hello()external returns(uint length,uint [3] memory array){
		//获取第三个元素的值
		uint A = book[2];
		book[2] = A * 2;//将原来第三个元素乘2获得新的第三个元素值并将其赋值
		return(book.length,book);
	}
}
```

之后运行程序，运行函数，返回结果：数组长度为3，数组元素为1，2，6。

##### 2、动态长度数组

动态长度数组相比固定长度数组，就是长度可以改变。

声明一个动态长度数组只需指定元素类型，不用指定数量，语法如下：

```solidity
type_name arrayName[];
```

动态长度数组初始化后是长度为0的空数组，可以用**push**方法，在数组末尾追加一个元素，也可以用**pop**方法删掉数组末尾一个元素。

其中push用法有两种，一种带参数push(x)，第二种不带参数push()，但不带参数就会在数组末尾添加一个默认值的元素。

下面是一个完整的示例代码：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract DonTaiValue{
	uint[] book = [uint(1),2,3];
	function Hello()external returns(uint length,uint[] memory array){
		book.push[6];
		book.push[];//增添三个元素
		book.push[4];
		book.pop();//删除数组中最后一个元素
		return (book.length,book);
	}
}
```

##### 3、new、delete操作

我们可以用new关键字来动态创建一个数组，具体代码示范如下：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract NewValue{
	function newArray()external pure returns(uint[] memory){
		uint[] memory arr = new uint[](3);//新建一个长度为3元素值都是默认值的arr数组
		arr[0] = 1;//将arr数组中第一个元素赋值为1
		return arr;
	}
}
```

通过new关键字创建的数组是一个动态长度数组，无法一次性进行赋值，只能逐一对其中元素进行赋予初值。

另外还有**delete**操作符可以对变量值重新初始化，前面说过的。

delete arr[i]是将第i个元素恢复为默认值，不是删除该元素！

```solidity
uint[] book = [uint(1),2,3];
delete book[0];//将book数组中第一个元素从1恢复到默认值0
```



#### B、字节和字符串

字节型是solidity中一种重要的数据类型。用于表示特定长度的字节序列，本质上是一个字节数组。

##### 1、固定长度字节型

按长度分为32种小类，使用bytes1、bytes2直到bytes32表示，固定长度字节型的变量声明如下：

```solidity
bytes1 myBytes1 = 0x12;//单个字节
bytes2 myBytes2 = 0x1234;//两个字节
bytes32 myBytes32 = 0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef;// 32个字节
```

对于固定长度字节型的变量使用方法和固定长度数组基本相同。

下面是一个完整的示例代码：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract GuDinBytes{
	function helloBytes()external pure returns(uint,bytes2,bytes1){
		bytes2 myBytes = 0x1212;//声明固定长度字节型变量为2
		bytes1 b = myBytes[0];//读取单个字节的值
		myBytes = 0x3456;//设置整个字节型变量的值
		return(myBytes.length,myBytes,b);
	}
}
```

##### 2、动态长度字节型

其变量使用方法和动态长度数组类似。

下面是个完整的示例代码：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract DongTaiBytes{
	function helloBytes()external pure returns(uint,bytes memory){
		bytes memory myBytes = new bytes(2);//创建动态长度字节型变量
		myBytes[0] = 0x12;//设置单个字节的值
		myBytes[1] = 0x34;
		return(myBytes.length,myBytes);
	}
}
```

最后运行代码，返回值是：2，0x1234。

##### 3、字符串

字符串是用来储存文本数据的类型。字符值用单引号或双引号来包括，类型用string表示。

```solidity
string public myString = "Hello World";
```

字符串与固定长度字节数组非常类似，它的值在声明后就不能变。

如果想对字符串中包含的字符进行操作，通常会将它转换为bytes类型。

solidity提供了字节数组bytes与字符串string之间的内置转换。

①将bytes转换成string

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract StringConvertor{
	function bytesToString()external pure returns(string memory){
		bytes memory myBytes = new bytes(2);
		myBytes[0] = 'o';
		myBytes[1] = 'k';
		return string(myBytes);
	}
}
```

②将string转换成bytes

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract bytesConvertor{
	function bytesToString()external pure returns(bytes memory){
		string memory myString = "Hello World";
		return bytes(myString);
	}
}
```



#### C、结构体

























































































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

