Dart的使用：


Dart中所有东西都是对象，包括数字、函数等。

一、关键字（56个）

33个保留字（不能使用保留字作为标识符）

关键字|-|-|-
-|-|-|-
if|superdo|switch|assert
else|in|this|enum|
is|throw|true|break
new|try|case|extends
null|typedef|catch|var
class|false|void|const
final|rethrow|while|continue
finally|return|with|for
default|||


其中内置标志符有：（17个）

关键字|-|-|-
-|-|-|-
abstract|deferred|as|dynamic
covariant|export|external|factory
get|implements|import|library
operator|part|set|static
typedef|||

其中Dart2相对于Dart1新增的,支持异步功能的关键字有：（6个）
关键字|-|-|-
-|-|-|-
async|async*|await|sync*
yield|yield*||


跟java相比，Dart特有的关键字有：（25个）
关键字|-|-|-
-|-|-|-
deferred|as|assert|dynamic
sync*|async|async*|in
is|await|export|library
external|typedef|factory|operator
var|part|const|rethrow
covariant|set|yield|get
yield*|||

二、变量和常量

（一）变量的声明，可以使用 var、Object 或 dynamic 关键字。

创建变量并初始化变量实例：

    var name = '张三' ;

变量存储引用。

* 1) 使用Object或dynamic关键字

    dynamic name = '张三';

调用的变量name包含对String值为“张三” 的对象的引用。
name推断变量的类型是String，但可以通过指定它来更改该类型。
如果对象不限于单一类型（没有明确的类型），请使用Object或dynamic关键字

    Object name = '张三';
    dynamic name = '李四';

* 2) 显式声明将被推断的类型

比如String,int等。

	//可以使用String显示声明字符串类型
    String name = '张三' ; //代替var name = '张三';

这个类型有很多，具体在下文有介绍。

（二）默认值

未初始化的变量的初始值为null（包括数字），因此数字、字符串都可以调用各种方法。

    //测试 数字类型的初始值是什么?
    int intDefaultValue;
    // assert 是语言内置的断言函数，仅在检查模式下有效
	// 在开发过程中， 除非条件为真，否则会引发异常。(断言失败则程序立刻终止)
	assert(intDefaultValue == null);
	print(intDefaultValue);//打印结果为null，证明数字类型初始化值是null

（三）Final and const

如果您从未打算更改一个变量，那么使用 final 或 const，不是var，也不是一个类型。
一个 final 变量只能被设置一次;const 变量是一个编译时常量。(Const变量是隐式的final。)
final的顶级或类变量在第一次使用时被初始化。


* 1) 被final或者const修饰的变量，变量类型可以省略。
	
	//可以省略String这个类型声明
	final name1 = "张三";
  //final String name1  = "张三";
    
  const name2 = "李四";
	//const String name2 = "李四";

* 2) 被 final 或 const 修饰的变量无法再去修改其值。
  final name1 = "张三";
	// 这样写，编译器提示：a final variable, can only be set once
	// 一个final变量，只能被设置一次。
	//name1 = "zhangsan";
	
	const name2 = "李四";
	// 这样写，编译器提示：Constant variables can't be assigned a value
	// const常量不能赋值
	// name2 = "lisi";
	
* 3) 注意：flnal 或者 const 不能和 var 同时使用，
	
	//这样写都会报错
	//final var name1 = "张三";
	//const var name2 = "李四";

* 4) 常量如果是类级别的，请使用 static const

    static const speed = 100;

* 5) 常量的运算

	const speed = 100; //速度（km/h）
	const double distance = 2.5 * speed; // 距离 = 速度 * 时间

	final speed2 = 100; //速度（km/h）
	final double distance2 = 2.5 * speed2; // 距离 = 速度 * 时间

* 6) const关键字不只是声明常数变量。您也可以使用它来创建常量值，以及声明创建常量值的构造函数。 任何变量都可以有一个常量值。

  // 注意: [] 创建的是一个空的list集合
  // const []创建一个空的、不可变的列表（EIL）。
  var varList = const []; // varList 当前是一个EIL
  final finalList = const []; // finalList一直是EIL
  const constList = const []; // constList 是一个编译时常量的EIL

  // 可以更改非final,非const变量的值
  // 即使它曾经具有const值
  varList = ["haha"];

  // 不能更改final变量或const变量的值
  // 这样写，编译器提示：a final variable, can only be set once
  // finalList = ["haha"];
  // 这样写，编译器提示：Constant variables can't be assigned a value  
  // constList = ["haha"];

* 7)只要任何插值表达式是一个计算结果为null或数字，字符串或布尔值的编译时常量，那么文字字符串就是编译时常量。(关于$表达式和不同的数据类型后面会讲解。)

// 这些是常量字符串
const aConstNum = 0;
const aConstBool = true;
const aConstString = 'a constant string';

// 这些不是常量字符串
var aNum = 0;
var aBool = true;
var aString = 'a string';
const aConstList = const [1, 2, 3];

const validConstString = '$aConstNum $aConstBool $aConstString';
//这样用就会报错：Const variables must be initialized with a constant value
// const常量必须用conat类型的值初始化。
// const invalidConstString = '$aNum $aBool $aString $aConstList';

三、特殊数据类型


Dart中支持以下特殊类型：

* numbers 数字
* strings 字符串
* booleans 布尔
* lists (also known as arrays) list集合（也称为数组）
* maps map集合
* runes (for expressing Unicode characters in a string) 字符（用于在字符串中表示Unicode字符）
* symbols 符号

（一）num 数字类型

num是数字类型的父类，有两个子类 int 和 double。
num类型包括基本的运算符，如+,-,/和*，位运算符，如>>，在int类中定义。
如果num和它的子类没有你要找的东西，math库可能会找到。比如你会发现abs(),ceil()和floor()等方法。

(1) int

int表示整数，int的取值不大于64位，具体取决于平台。在Dart VM上，值可以从 -2的63次方 到 2的63次方减1.

整数是没有小数点的数字。示例如下：

    int intNum1 = 10 ;
    print(intNum1);
    int intNum2 = 0xDEADBEEF ;
    print(intNum2);
 


(2) double

64位（双精度）浮点数，如IEEE 754标准所规定。

如果一个数字包含一个小数，那么它就是一个double类型。示例如下：

	double doubleNum1 = 1.1;
	print(doubleNum1);
	double doubleNum2 = 1.42e5;
	print(doubleNum2);

（二）String 字符串

Dart里面的String是一系列 UTF-16代码单元。


* 1) 您可以使用单引号或双引号来创建一个字符串。

	String str1 = '单引号基本使用demo.';
	String str2 = "双引号基本使用demo.";

* 2) 单引号或者双引号里面嵌套使用引号。

	//单引号里面嵌套单引号，必须在前面加反斜杠
	String str3 = '双引号里面有单引号it\'s，必须在前面加反斜杠.';
	//双引号里面嵌套单引号（正常使用）
	String str4 = "双引号里面有单引号it's.";
	//单引号里面嵌套双引号（正常使用）
	String str5 = '单引号里面有双引号，编程开发初学必备语句"hello world"';
	//双引号里面嵌套双引号，必须在前面加反斜杠
	String str6 = "双引号里面有双引号，编程开发初学必备语句\"hello world\"";

* 3) 多个字符串相邻中间的空格问题：

经测试发现，除了单引号嵌套单引号或者双引号嵌套双引号不允许出现空串之外，其余的几种情况都是可以运行的。
示例如下：

	// 这个会报错
    //String blankStr1 = 'hello''''world';
    // 这两个运行正常
    String blankStr2 = 'hello'' ''world'; //结果： hello world
    String blankStr3 = 'hello''_''world'; //结果： hello_world

    // 这个会报错
    //String blankStr4 = "hello""""world";
    // 这两个运行正常
    String blankStr5 = "hello"" ""world"; //结果： hello world
    String blankStr6 = "hello""_""world"; //结果： hello_world

    //单引号里面有双引号，混合使用运行正常
    String blankStr7 = 'hello""""world'; //结果： hello""""world
    String blankStr8 = 'hello"" ""world'; //结果： hello"" ""world
    String blankStr9 = 'hello""_""world'; //结果： hello""_""world

    //双引号里面有单引号，混合使用运行正常
    String blankStr10 = "hello''''world"; //结果： hello''''world
    String blankStr11 = "hello'' ''world"; //结果： hello'' ''world
    String blankStr12 = "hello''_''world"; //结果： hello''_''world

* 4) 您可以使用相邻字符串文字或+ 运算符连接字符串：

例如：

    > 直接把相邻字符串写在一起，就可以连接字符串了。

    String connectionStr1 =  '字符串连接'
    '甚至可以在'
    '换行的时候进行。';

    > 用+把相邻字符串连接起来。

    String connectionStr2 =  '字符串连接'
      + '甚至可以在'
      + '换行的时候进行。';

    > 使用单引号或双引号的三引号：
    
    String connectionStr3 = ''' 
      你可以创建
      像这样的多行字符串。
      ''' ;

    String connectionStr4 = """这也是一个
      多行字符串。""";

* 5) 关于转义符号的使用

声明raw字符串（前缀为r），在字符串前加字符“r”，或者在\前面再加一个\，
可以避免“\”的转义作用，在正则表达式里特别有用

举例如下：

    print(r"换行符：\n"); //这个结果是 换行符：\n
    print("换行符：\\n"); //这个结果是 换行符：\n
    print("换行符：\n");  //这个结果是 换行符：

* 6) 可以使用${表达式}将表达式的值放入字符串中。如果表达式是标识符，则可以跳过{}。
为了获得对应于object的字符串，Dart调用object的toString()方法。

例如：

    String replaceStr1 = '字符串连接';
    print('$replaceStr1'
        + '甚至可以在换行的时候进行。' == '字符串连接'
        + '甚至可以在换行的时候进行。');

    String replaceStr2 = 'Android Studio';
    print('你知道' +
        '${replaceStr2.toUpperCase()}'
          + '最新版本是多少吗？' ==
          '你知道ANDROID STUDIO最新版本是多少吗？');

注：
    ==操作符测试两个对象是否相等。如果两个字符串包含相同的代码单元序列，那么它们是等效的。

有关如何在字符串中表示Unicode字符的详细信息，请参见下文“字符”）。
关于String的更多详细的api用法，后文会专门讲解。

（三）bool 布尔值

为了表示布尔值，Dart有一个名为的类型bool。
只有两个对象具有bool类型：true和false，它们都是编译时常量。

Dart的类型安全意味着您不能使用if（nonbooleanValue）或assert（nonbooleanValue）等代码。 
相反，Dart使用的是显式的检查值，如下所示：

    // 检查是否为空字符串
    var fullName = '';
    assert(fullName.isEmpty);

    // 检查0
    var hitPoints = 0;
	assert(hitPoints <= 0);

	// 检查是否为null
	var unicorn;
	assert(unicorn == null);

	// 检查是否为NaN
	var iMeantToDoThis = 0 / 0;
	assert(iMeantToDoThis.isNaN);

assert 是语言内置的断言函数，仅在检查模式下有效
在开发过程中， 除非条件为真，否则会引发异常。(断言失败则程序立刻终止)。

（四）list 集合（也称为数组）

几乎所有编程语言中最常见的集合可能是数组或有序对象组。 
在Dart中，数组是List对象，因此大多数人只是将它们称为List。
Dart list文字看起来像JavaScript数组文字。 以下是一个简单的Dart列表：

    //创建一个int类型的list
    List list = [10, 7, 23];
    print(list);

要创建一个编译时常量const的list，示例如下：

    List constantList = const[10,3,15];
    print(constantList);   


注意事项：

    1.可以直接打印list包括list的元素，list也是一个对象。但是java必须遍历才能打印list，直接打印是地址值。
    2.和java一样list里面的元素必须保持类型一致，不一致就会报错。
    3.和java一样list的角标从0开始。

关于list的相关api介绍，后面会做详细讲解。


（五）map集合

一般来说，map是将键和值相关联的对象。键和值都可以是任何类型的对象。
每个键只出现一次，但您可以多次使用相同的值。Dart支持map由map文字和map类型提供。

* 1) 两种创建方式：

直接声明，用{}表示，里面写key和value，每组键值对中间用逗号隔开

    Map companys = {'first': '阿里巴巴', 'second': '腾讯', 'fifth': '百度'};
    print(companys);//打印结果 {first: 阿里巴巴, second: 腾讯, fifth: 百度}

先声明，再去赋值。（以变几种形式都是可以的。）

    Map companys1 = new Map();
    companys1['first'] = '阿里巴巴';
    companys1['second'] = '腾讯';
    companys1['fifth'] = '百度';
    print(companys1);
    //打印结果 {first: 阿里巴巴, second: 腾讯, fifth: 百度}

    var companys2 = new Map();
    companys2['first'] = '阿里巴巴';
    companys2['second'] = '腾讯';
    companys2['fifth'] = '百度';
    print(companys2);
    //换成var 打印结果是一样的 {first: 阿里巴巴, second: 腾讯, fifth: 百度}

    Map companys3 = new Map();
    companys3["first"] = "阿里巴巴";
    companys3["second"] = "腾讯";
    companys3["fifth"] = "百度";
    print(companys3);
    //换成双引号 打印结果 {first: 阿里巴巴, second: 腾讯, fifth: 百度}

    var companys4 = new Map();
    companys4["first"] = "阿里巴巴";
    companys4["second"] = "腾讯";
    companys4["fifth"] = "百度";
    print(companys4);
    //换成双引号,换成var 打印结果 {first: 阿里巴巴, second: 腾讯, fifth: 百度}

* 2) 添加元素。格式 变量名[key] = value; 

注意：key可以是不同类型。

  //添加一个新的元素，key为 4  value为 华为
  companys[4] = '华为';
  print(companys);//打印结果 {first: 阿里巴巴, second: 腾讯, fifth: 百度, 4: 华为}

* 3) 修改元素

  //把key为first的元素对应的value改成 alibaba
  companys['first'] = 'alibaba';
  print(companys);//打印结果 {first: alibaba, second: 腾讯, fifth: 百度, 4: 华为}

* 4) 注意事项

  // map里面的value可以相同
  Map company1 = {
    'first': '阿里巴巴', 'second': '腾讯', 5: '百度','new key': '阿里巴巴'};
  print(company1);
  //打印结果 {first: 阿里巴巴, second: 腾讯, 5: 百度, new key: 阿里巴巴}

  Map company2 = new Map();
  company2['first'] = '阿里巴巴';
  company2['second'] = '腾讯';
  company2['fifth'] = '百度';
  company2['new key'] = '阿里巴巴';  // map里面的value可以相同
  company2['new key2'] = '';  // map里面value可以为空字符串
  company2['new key3'] = null;  // map里面的value可以为null
  print(company2);
  //打印结果 {first: 阿里巴巴, second: 腾讯, fifth: 百度, new key: 阿里巴巴, new key2: , new key3: null} 

* 5）要创建一个编译时常量const的map，请在map文字之前添加const：

    final fruitConstantMap = const {2: 'apple',10: 'orange',18: 'banana'};

* 6) 小结： 

    1.创建map有两种方式。
    2.map的key类型不一致也不会报错。
    3.添加元素的时候，会按照你添加元素的顺序逐个加入到map里面，哪怕你的key，比如分别是 1,2,4，看起来有间隔，事实上添加到map的时候是{1:value,2:value,4:value} 这种形式。
    4.添加的元素的key如果是map里面某个key的英文，照样可以添加到map里面。
    5.map里面的key不能相同。但是value可以相同,value可以为空字符串或者为null。

关于map的相关api介绍，后面会做详细讲解。


（六）runes (for expressing Unicode characters in a string) 字符（用于在字符串中表示Unicode字符）


Unicode为世界上所有的书写系统中使用的每个字母，数字和符号定义了唯一的数值。 

由于Dart字符串是UTF -16代码单元的序列，所以在字符串中表达32位Unicode值需要特殊的语法。
表示Unicode代码点的常用方法是\uXXXX，其中XXXX是一个4位十六进制值。 

例如，心形字符（♥）是\u2665。 
要指定多于或少于4个十六进制数字，请将该值放在大括号中。 例如，笑的表情符号（😆）是\u{1f600}。

String类有几个属性可以用来提取符文信息。 codeUnitAt和codeUnit属性返回16位代码单元。 
使用Runes属性来获得一个string的符文。

以下示例说明了符文，16位代码单元和32位代码点之间的关系。

    var clapping = '\u{1f44f}';
    print(clapping);
    print(clapping.codeUnits);
    print(clapping.runes.toList());

    //这里使用String.fromCharCodes方法显示字符图形
    //String的详细api后面会具体讲解。
    Runes input = new Runes(
        '\u2665  \u{1f605}  \u{1f60e}  \u{1f47b}  \u{1f596}  \u{1f44d}');
    print(new String.fromCharCodes(input));

（七）symbols 符号

有两个 #radix   #bar  可以使用
官网文档没看明白这个具体是做什么的。。暂且放着。
这个用的很少，基本用不到的。

四、运算符 operators
（一）算术运算符：
操作者 |含义
-|-
+ |加
– |减去
-expr |一元减号，也称为否定（反转表达的符号）
* |乘
/ |除
~/ | 除法，返回一个整数结果
% |获取整数除法的余数（模）

示例：




（二）赋值运算符：
（三）关系运算符：
（四）逻辑运算符：
（五）位运算符：
（六）位移运算符：
（七）三元运算符：




五、函数 Function

六、控制流程语句 Control flow statements
七、异常 Exceptions
八、类 Classes
九、泛型 Generics
十、库和可见性 Libraries and visibility
十一、异步支持 Asynchrony support
十二、生成器 Generators
十三、可调用的（Callable）类
十四、隔离 Isolates
十五、类型定义 Typedefs
十六、元数据 Metadata
十七、注释 Comments
十八、概要 Summary