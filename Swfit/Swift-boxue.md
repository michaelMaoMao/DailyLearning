# Swift-boxue


## Tuple

很多时候，我们需要把多个不同类型的值，打包成一个单位处理。例如，返回一个HTTP状态信息：

*   状态码: 200; 状态消息: HTTP OK
*   状态码: 404; 状态消息: File not found

或者，返回一条数据库用户信息记录：`姓名: Mars; 工号: 11; 电子邮件: 11@boxue.io`。在Swift里，我们可以使用Tuple 来很方便的处理类似的问题。

### 定义一个Tuple

我们使用下面的方式，来定义一个Tuple：

```
(value1, value2, value3...)

```

例如，定义我们开始提到的HTTP状态码：

```
//: #### Define a tuple

let success = (200, "HTTP OK")
let fileNotFound = (404, "File not found")

```

我们还可以给Tuple中的每一个数据成员指定一个名字，例如，定义一个表达用户信息记录的Tuple：

```
(name1: value1, name2: value2, name3: value3...)

//: #### Define a tuple
let me = (name: "Mars", no: 11, email: "11@boxue.io")

```

### 访问Tuple中的数据成员

定义好Tuple之后，我们可以使用下面的方式访问Tuple中的数据成员：

```
//: #### Access tuple content

success.0
success.1

fileNotFound.0
fileNotFound.1

```

如果我们在定义Tuple时，指定了Tuple成员的名字，我们就可以像下面这样访问这些数据成员：

```
//: #### Access tuple content

me.name
me.no
me.email

```

### Tuple Decomposition

我们在定义Tuple的时候，还可以把一个Tuple的值，一一对应的拆分到不同的变量上，这叫做Tuple Decomposition。例如，对于之前定义过的`success`，我们可以这样定义一个新的Tuple：

```
var (successCode, successMessage) = success

print(successCode) // 200
print(successMessage) // HTTP OK

```

之后，就可以直接访问`successCode`和`successMessage`的值了。这可以提高我们处理Tuple成员时的代码可读性。但要说明的是，我们这里是使用`success`的值，构建了一个新的Tuple，因此修改`succeCode`或`successMessage`的值，不会影响到原来的`success`。

例如，我们修改`successCode`：

```
successCode = 201

success // (200, "HTTP OK")

```

从结果我们可以看到，之前的`success`的值没有被修改。另外，如果我们只是想对应到Tuple中特定的成员，而忽略其它成员，我们可以使用下划线'_'来代表那些不需要被对应的成员。例如：

```
let (_, errorMessage) = fileNotFound
print(errorMessage)

```

### Tuple type

每一个Tuple的类型，都是由Tuple中所有数据成员一起决定给的。例如，对于一开始我们定义的success和me，它们的类型就分别是：`(Int, String)`和`(String, Int, String)`。当我们需要用type annotation定义一个Tuple的时候，我们可以这样写：

```
var redirect: (Int, String) = (302, "temporary redirect")

```

### Tuple comparison

当我们比较两个Tuple类型的变量时，要遵循下面的规则：

首先，只有元素个数相同的Tuple变量之间，才能进行比较。例如，下面的代码会引发编译错误：

```
let tuple12 = (1, 2)
let tuple123 = (1, 2, 3)

tuple2 < tuple3

```

![tuple compare error](https://o8lw4gkx9.qnssl.com/packdata-by-tuple-1@2x.png)

从上面的结果就能看到，包含两个`Int`的Tuple不能和包含三个`Int`的Tuple进行比较。

其次，当Tuple中元素个数相同时，比较是按照Tuple中元素的位置从前向后依次进行的：

*   如果Tuple中，相同位置的两个元素相等，则继续比较下一个位置的两个元素，并根据第一个同一位置不相等的两个元素的大小关系，确定两个Tuple变量的关系；
*   如果两个Tuple中所有位置的元素都相等，则两个Tuple变量相等；

因此，对于下面这个例子，`tuple11 < tuple12`的结果是`true`：

```
let tuple11 = (1, 1)
let tuple12 = (1, 2)

tuple11 < tuple12 // true

```

但是，有一点要说明的是，我们只可以对最多包含6个元素的Tuple变量进行比较，超过这个数量，Swift会报错。例如对于下面这段代码：

```
let largeTuple1 = (1, 2, 3, 4, 5, 6, 7)
let largeTuple2 = (1, 2, 3, 4, 5, 6, 7)

largeTuple1 == largeTuple2 // Error !!!

```

编译器就会提示类似下面这样的错误：

![tuple compare error](https://o8lw4gkx9.qnssl.com/packdata-by-tuple-2@2x.png)



## 基本操作符（Basic operators）

操作符，是一个编程语言中完成各种运算不可或缺的元素。Swift中的大部分操作符，都符合我们对各种运算的理解。因此，在这篇文档里，我们将带着大家快速过一遍Swift中的各种操作符，并对其中大家可能不太熟悉的部分，做一些特别的说明。


### 赋值操作符

这个最简单，我们之前已经用过多次，等号右边的值赋值给等号左边的变量：

```
//: #### Basic assignment
let a = 20
var b = 10

```

### 基本算术运算操作符

```
let sum = a + b
let sub = a - b
let mul = a * b
let div = a / b
let mod = a % b

```

> “Swift 3不再允许浮点数取模。例如：8 ％ 2.5这样的写法在Swift 3中将会报错。如果要对浮点数取模，只能这样： `8.truncatingRemainder(dividingBy: 2.5)`。”

### 复合运算操作符

Swift还支持把赋值和算数运算符组合起来：

```
//: #### Compound assignment

b += 10 // b = b + 10
b -= 10 // b = b - 10
b *= 10 // b = b * 10
b /= 10 // b = b / 10
b %= 10 // b = b % 10

```

> “Swift不会把数字自动转换成`Bool`类型。在需要`Bool`值的地方，你必须明确使用一个`Bool`变量。”
> 
> “Swift 3中不再支持自增（++）和自减（--）操作符，使用它们的前缀和后缀版本都会得到一个编译器错误。因此，需要+1/-1的时候，只能使用`b += 1`和`b -= 1`来实现。”

### 比较操作符

Swift支持以下常用的比较操作：

```
//: #### Comparison
let isEqual     = sum == 10
let isNotEqual  = sum != 10
let isGreater   = sum >  10
let isLess      = sum <  10
let isGe        = sum >= 10
let isLe        = sum <= 10

```

除此之外，Swift还支持两个用于比较对象引用的操作符：Identity operator，它们用来判断两个操作数是否引用同一个对象，我们在后面讲到面向对象编程的时候，会进一步提到这两个操作符。

```
//: Identity operator

//===
//!==

```

### 三元操作符

```
/*
* if condition {
*     expression1
* }
* else {
*     expression2
* }
*
*/

let isSumEqualToTen = isEqual ? "Yes" : "No"

```

在Swift里，一些“日常”的`if...else...`判断，可以使用下面的三元操作符来替代：`cond ? expr1 : expr2`。

### Nil Coalescing Operator

这是一个Swift特有的操作符，用来处理和Optional有关的判断：

```
// opt != nil ? opt! : b

var userInput: String? = "A user input"
let value = userInput ?? "A default input"

```

如果`opt`是一个optional，当其不为`nil`时，就使用optional变量自身的值，否则，就使用`??`后面的“默认值”。

### Range operator

#### 闭区间range operator

我们用下面的方式表达一个包含begin和end的闭区间：`begin ... end`：

```
//: Closed range operator
// begin...end

for index in 1...5 {
    print(index)
}

```

##### 半开半闭区间range operator

我们用下面的方式表达一个[begin, end)的半开半闭区间：`begin ..< end`：

```
//: Half-open range operator
// begin..<end [begin, end)

for index in 1..<5 {
    print(index)
}

```

### 逻辑运算符

Swift支持三种常用的逻辑运算：NOT，AND和OR。它们都返回一个`Bool`：

```
//: #### Logic operator

let logicalNot = !isEqual
let logicalAnd = isNotEqual && isLess
let logicalOR  = isGreater  || (isLess && isLe)

```


## 使用“Markdown方言”编写代码注释

作为伴随Swift 3发布的API设计指南中的要求，**使用Swift Markdown为代码编写注释**已经进一步从一个道义提升为了一种行为准则。一方面，Markdown对开发者来说，足够熟悉，学习难度并不高；另一方面，Xcode可以在Playground和代码提示中，对Markdown注释进行漂亮的渲染，让代码在开发者之间交流起来更加容易。既然如此，我们就通过这段视频，带着大家快速过一遍Swift中的Markdown注释机制，了解它们在Xcode哪些地方会被渲染出来。

### 从一个包含playground文件的Single View Application开始

为了演示Markdown注释在Xcode中的各种效果，我们创建一个Single View Application，并向其中添加了一个MyPlayground文件以及一个包含`struct IntArray`类型的源代码文件。因为，在Xcode在Playground和项目源代码中，使用了两种不同的Markdown注释格式，我们要分别了解它们。

为了能快速切换Markdown注释在Playground中的渲染效果，我们可以按`Command + ,`打开属性对话框，选择“Key Bindings”

![Key bindings](https://o8lw4gkx9.qnssl.com/br-markdown-comment-1@2x.jpg)

在“Filter”中，输入`Show rendered`，Xcode会为我们自动过滤出筛选的菜单项目，“Show rendered Markup”用于在Playground里，切换Markdown注释的渲染。双击右侧的空白，为它设定一个快捷键，例如：`option + M`：

![Show rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-2@2x.jpg)

这样，我们就能通过设置的快捷键在Playground中快速切换Markdown的渲染效果了。接下来，我们就来看各种常用的Markdown用法。

* * *

#### Playground

*   包含Markdown的单行注释用`//:`表示：

```
//: # Heading 1

```

*   包含Markdown的多行注释用下面的代码表示：

```
/*:
  * item1
  * item2
  * item3
 */

```

按`Option + M`，Playground就会自动为我们渲染注释了：

![Rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-3@2x.jpg)

* * *

#### Symbol documentation

如果是在Xcode项目的源代码中：

*   包含Markdown的单行注释用`///`表示：

```
/// A **demo** function
func demo() {}

```

*   包含Markdown的多行注释用下面的代码表示：

```
/**
  * item1
  * item2
  * item3
*/
func demo1() {
}

```

我们这样添加的注释会显示在哪呢？主要有两个地方，Apple管它们叫做symbol documentation，我们可以用两种方式来查阅它：

*   把光标放到`demo1`所在的行上，按住`option`点一下，就会弹出这个函数的说明，可以看到Xcode已经把markdown注释渲染了；

![Rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-4@2x.jpg)

*   按`Option + Command + 2`打开Quick Help Inspector，保持光标在demo1()所在行，同样，我们可以看到被渲染过的Markdown注释；

![Rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-5@2x.jpg)

> 简单来说，在Playground里编写markdown注释时，注释起始的第三个字符用`:`，在项目源代码中编写markdown注释时，注释起始的第三个字母分别用`/`和`*`。

* * *

#### 常用的注释范式

接下来，我们看来一些在Playground和项目代码中经常会用到的注释范式。至于其中用到的Markdown语法，大家应该都比较熟悉，我们就不一一去解释了。大家也可以在[这里查看Apple官方的Swift markdown语法说明](https://developer.apple.com/library/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1)。

* * *

##### 标记重要事项

当我们用Playground告知开发者代码中的重要事项时，可以采用下面这种类似的方式进行注释：

```
/*:
  > # IMPORTANT: something important you want to mention:
  A general descripiton here.
  1\. item1
  1\. item2
  1\. item3
  ---
  [More info - Access boxueio.com](https://boxueio.com)
 */

```

*   一行简短的重要提示标题；
*   一段内容摘要；
*   一个注意事项列表；
*   一个提供更多内容的链接；

在Playground里，上面的注释可以被渲染成这样：

![Rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-6@2x.jpg)

如果我们要把上面的注释放在项目代码里，出了要使用`/**`开始外，我们还要去掉第一行的`>`，因为Quick Help不支持这样的Markdown：

```
/**
  # IMPORTANT: something important you want to mention:
  A general descripiton here.
  1\. Start with
  1\. Write anything important you want to emphasize
  1\. End with  at a new line.
  ---
  [More info - Access boxueio.com](https://boxueio.com)
 */

```

这样，它看起来，就是这样的：

![Rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-7@2x.jpg)

有关函数自身的注释范式，稍后我们会看到。接下来，我们先看一个简化注释输入的方法。

* * *

##### 在Playground之间跳转

有时，为了演示一个项目的不同用法和功能，我们可能会在项目中使用多个Playground文件。为了方便在注释中浏览，我们可以在Playground markdown注释中，添加文件跳转链接。

选中项目中的MyPlayground，点击右键，选择“New Playground Page”，添加2个新的page进来，我们把这些页面分别命名成Page1 / Page2 / Page3。

在新添加进来的page2和page3里，先分别添加一个标题注释以方便区分它们。然后我们可以看到，在Playground页面的头部和尾部，Xcode已经为我们自动添加了两个链接：

```
//: [Previous](@previous)

//: [Next](@next)

```

按`Option + M`，切换到渲染模式，分别点击页面上的Previous和Next链接，就会发现可以在页面间前后跳转了。实际上，这里用了两个关键字，`@previous`和`@next`，Xcode会自动把它们渲染成跳转到项目文件列表中前、后两个文件的链接。

当然，我们也可以实现跨文件跳转，打开Page1，这次在括号里写上要跳转到的目标Playground页面的名字：

```
//: [To Page3](Page3)

```

再切换到渲染模式，点击“To Page3”，就可以跳转到相应的Playground页面了。接下来，我们来看如何通过Xcode Code Snippet Library简化复杂注释的输入。

* * *

### Code Snippet Library

首先，把之前我们用过的注释块，抽象成一个内容模板：

```
/*:
  > # IMPORTANT: <#something important#>
  <#General description#>
  1\. <#item1#>
  1\. <#item2#>
  1\. <#item3#>
  ---
  [More info - <#ref#>](<#Link#>)
*/

```

基本内容和之前还是一样的，只不过，我们把其中需要输入内容的地方用一对`<# ... #>`包围了起来，这用于告诉Xcode，这些内容是需要每次用户单独输入的。

其次，我们选中要添加的代码块，先不要着急拖动，按住等待一会儿，直到鼠标从输入状态变回指针状态；

第三、把代码块拖动到Code Snippet Library：

![Rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-8@2x.jpg)

这样，在Code Snippet Library里，就会多出来一项，表示我们新添加的代码片段，在图标的左下角，左右一个“User”字样，表示这是我们自定义的代码片段：

![Rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-9@2x.jpg)

第四、点击“Edit”按钮，编辑一下这个代码片段：

![Rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-10@2x.jpg)

其中：

*   `Title`表示代码片段的名称；
*   `Summary`表示代码片段的简单说明；
*   `Platform`表示代码片段在 iOS / macOS / watchOS / tvOS / All 中使用；
*   `Language`表示代码片段生效的语言；
*   `Completion Shortcut`表示如何调出这个代码片段，在这里我们选择输入`importantnote`，当然你可以设置成任何方便使用的内容；
*   `Completion Scopes`表示上面设置的`Completion Shortcut`生效范围，All表示在任意位置都生效；

设置完成后，点击"Done"。这样当我们在Swift代码里输入`importantnote`的时候，就能调出需要的注释片段，我们只要直接设置其中的内容就好了。

接下来，我们将看到一些常用的注释范式，它不仅可以让代码易于维护和交流，就像API设计指南中的描述的那样，可以有效的激发我们的设计灵感。

* * *

#### 标记自定义类型的常用范式

对于一个自定义类型来说，我们要在注释中说明以下问题：

*   一句话描述；
*   类型主要功能；
*   常用的初始化方法以及拷贝语义；
*   补充说明；

而对这些问题的阐述，正是我们设计这个类型的过程。因此，在编码前设计注释文档，可以很好的帮我们整理设计思路。

例如，我们创建一个包含整数的`struct IntArray`，在Playground里添加下面的注释：

```
/*:
 `IntArray` is a C-like random access collection of integers.

 ## Overview
 An `IntArray` stores values of integers in an ordered list.
 The same value can appear in an IntArray multiple times at
 different positions.

 ## Initializers
 You can create an IntArray in the following ways:

    // An empty IntArray
    var empty: IntArray = []

    // Initialzied by an array literal
    var odds: IntArray = [0, 2, 4, 6, 8]

    // Initialized by a default value
    var tenInts: IntArray = IntArray(repeating: 0, count: 10)

 ## Value semantics
 - important:
 `IntArray` object perform value type semantics. But we have the COW optimization.

 Like all value types, `IntArray` use a COW optimization.
 Multiple copies of `IntArray` share the same storage as long as
 none of the copies are modified.

 ---

 - note:
 Check [Swift Standard Library](https://developer.apple.com/reference/swift/array)
 for more informaton about arrays.
 */

```

上面的注释被Xcode渲染出来是这样的：

![Rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-11@2x.jpg)

而把这段注释移植到Xcode项目代码中，在Quick Help中看到的结果是这样的：

![Rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-12@2x.jpg)

当然，你可以任意在注释里添加希望其它开发者了解的内容，例如访问、存取、修改数组的方法等。也可以通过Snippet library，把它保存起来。

在上面的注释里，有几个用法是要特别说明下的：

*   我们可以在注释中使用`single line of code`来插入单行代码；
*   在注释中插入代码块时，**代码块的缩进要和当前最近的一个内容缩进有4个以上的空格**，否则Xcode不会识别；
*   我们在注释中使用了两个用`-`开始的标记，它们叫做callout，实际上你可以选择使用加号、减号或乘号来表示一个callout。Xcode可以识别它们，并突出显示其中的内容。大家可以在[这里](https://developer.apple.com/library/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/MarkupFunctionality.html#//apple_ref/doc/uid/TP40016497-CH54-SW1)找到所有的callout元素列表。要说明的是，并不是所有callout都可以同时在Playground和Quick Help中使用，选择的时候，要注意这点；
*   最后，我们可以**使用三个及以上的**`-`，表示一条分割线，用来区分正文和内容引用的部分；

* * *

#### 标记函数或方法的常用范式

通常，对一个方法的描述，更多是用在Quick Help里。对于一个函数来说，最重要的内容无非有以下：

*   一句话功能描述；
*   常见应用场景；
*   参数；
*   返回值；
*   时间复杂度；

如果我们要在上面`IntArray`里，添加一个“返回不包括末尾N个元素的IntArray”的方法：

```
public func dropLast(_ n: Int) -> IntArray

```

它的注释可以是这样的：

```
/// Returns a subsequence containing all but the specified number of final
/// elements.
///
/// If the number of elements to drop exceeds the number of elements in the
/// collection, the result is an empty subsequence.
///
///     let numbers = [1, 2, 3, 4, 5]
///     print(numbers.dropLast(2))
///     // Prints "[1, 2, 3]"
///     print(numbers.dropLast(10))
///     // Prints "[]"
///
/// - Parameter n: The number of elements to drop off the end of the collection.
///   `n` must be greater than or equal to zero.
///
/// - Returns: A subsequence that leaves off `n` elements from the end.
///
/// - Complexity: O(*n*), where *n* is the number of elements to drop.

```

通常，多行的代码注释也可以使用这种多个单行注释拼接的形式来编写，因为有时多行缩进的不同，会让整个注释从头部看起来比较混乱（大家可以对比下前面我们对`IntArray`的注释），而使用多个`///`则看起来会整齐一些。

在这个例子里，我们使用了三个callout，分别表示了方法的参数、返回值和时间复杂度，**如果算法复杂度是O(1)，那默认是可以省略的**。

在Quick Help里，它看上去是这样的：

![Rendered markdown](https://o8lw4gkx9.qnssl.com/br-markdown-comment-13@2x.jpg)

* * *

#### 标记属性的常用范式

关于属性的注释，我们只强调一点，就是对于computed property来说，**如果它的算法复杂度不是O(1)，必须在注释中予以说明**。因为对于绝大多数人来说，不会预期访问一个属性会带来严重的性能开销。

以上，就是在Swift 3 API设计指南中，关于注释的内容。**在编码前，用写文档的方式来编写注释**，可能在初期会让我们觉得不适应，或者觉得没必要，但至少在设计一个新的类型或方法时，尝试着去做它们，它会让你明确类型表达的语意，理清方法功能的边界，进而让你的代码，更加易于理解和交流。

[](https://boxueio.com/series/swift-up-and-running/ebook/5)



### 字符串的处理




```swift

//: # Markdown
/*:
 > # IMPORTANT: something important you want to mention:
 A general descripiton here.
 1. item1
 1. item2
 1. item3
 ---
 [More info - Access boxueio.com](https://boxueio.com)
 */



var hello = "hello"
//print(hello.substring(to: 1))
print(hello.startIndex)


String(hello.characters.prefix(3))

var mixStr = "Swift很有趣"

if let index = mixStr.characters.index(of: "很") {
    mixStr.replaceSubrange(cnIndex ..< mixStr.endIndex, with: " is interesting")
}


```

#### Map函数
 map：可以对数组中的每一个元素做一次处理

