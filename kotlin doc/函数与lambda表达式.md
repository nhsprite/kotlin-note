### 函数声明
kotlin中的函数由fun关键字声明：
```
fun double(x: Int): Int{
    return 2 * x
}
```

### 默认参数
函数参数可以有默认值，当省略相应的参数时使用默认值。可以减少函数的重载亮。

覆盖方法总是使用与基类型方法相同的默认参数值。当覆盖一个带有默认参数的方法时，必须从签名中省略默认参数值。
```
open class A {
    open fun foo(i: Int = 10) { …… }
}

class B : A() {
    override fun foo(i: Int) { …… }  // 不能有默认值
}
```

### 可变参数Varargs
函数的参数（通常是最后一个）可以用varargs修饰符标记：
```
fun <T> asList(vararg ts: T): List<T> {
    val result = ArrayList<T>()
    for (t in ts) // ts is an Array
        result.add(t)
    return result
}
```
允许将可变数量的参数传递给函数
```
val list = asList(1,2,3)
```

星号操作符：
```
fun foo(vararg strings: String){}

foo(strings = *arrayOf("a", "b", "c"))
```

### 局部函数
Kotlin支持局部函数，即一个函数在另一个函数内部（主要用于解决Java中函数抽离的问题）：
```
fun dfs(graph: Graph) {
    fun dfs(current: Vertex, visited: Set<Vertex>) {
        if (!visited.add(current)) return
        for (v in current.neighbors)
            dfs(v, visited)
    }

    dfs(graph.vertices[0], HashSet())
}
```
局部函数可以访问外部函数（即闭包）的局部变量

### 成员函数
成员函数是定义在类或者对象内部的函数（即平时Java中常见的函数）

### 泛型函数
泛型函数可以由泛型参数，通过在函数名前使用尖括号指定。
```
fun <T> singletonList(item: T): List<T> {
    // ……
}
```

### 尾递归特性
这允许一些通常用循环的算法改用递归函数来写，而无堆栈溢出的风险。用tailrec关键字标记。

> 没啥卵用，垃圾特性

### 高阶函数
高阶函数是指将函数作为参数或返回值的函数。

kotlin中有一个约定，如果函数的最后一个参数是一个函数，并且你传递一个lambda表达式作为相应的参数，你可以在圆括号之外指定它。

另一个有用的约定是，如果函数字面值只有一个参数，那么它的声明可以省略（连同->），其名称是it。

如果Lambda表达式的参数未使用，那么可以用下划线取代其名称。

### Lambda表达式与匿名函数
一个Lambda表达式或匿名函数是一个"函数字面值"，即一个未声明的函数，但立即作为表达式传递。

### 函数类型
对于接受另一个函数作为参数的函数，我们必须为该参数指定函数类型。例如：
```
fun <T> max(collection: Collection<T>, less: (T, T) -> Boolean): T? {
    var max: T? = null
    for (it in collection)
        if (max == null || less(max, it))
            max = it
    return max
}
```
参数less的类型是（T,T）-> (Boolean),即一个接受俩个类型T的参数并返回一个布尔值的函数。

声明一个函数类型的变量：
```
val compare: (x: T, y: T) -> Int = ……

```
如要声明一个函数类型的可空变量，请将整个函数类型括在括号中并在后面加上问号：
```
var sum: ((Int, Int) -> Int)? = null
```

### lambda表达式语法
lambda表达式的完整语法,即函数的字面值如下：`
```
val sum = {x: Int, y: Int -> x + y}
```
lambda表达式总是被大括号括着,完整语法形式的参数声明放在大括号内，并有可选的类型标注，函数体跟在一个->符号之后，如果推断出该lambda的返回类型不是Unit，那么该lambda主体的最后一个表达式会被视为返回值。

一个完整的lambda表达式:
```
val sum: (Int, Int) -> Int = {x, y -> x + y}
```

### 匿名函数
lambda表达式语法缺少指定函数的返回类型的能力。如果确实需要指定类型，可以使用匿名函数:
```
fun(x: Int, y: Int): Int = x + y;
```

匿名函数的返回类型推断机制与正常函数一样:对于具有表达式函数体的匿名函数将自动推断返回类型；对于具有代码块函数体的匿名函数必须显示指定（或返回类型为Unit）

匿名函数参数总是在括号内传递，允许将函数留在圆括号外的简写语法仅适用于lambda表达式。

一个不带标签的return语句总是用在fun关键字声明的函数中返回。这意味着lambda表达式中的return语句将从包含它的函数返回，而匿名函数中的return将从匿名函数自身返回。

### 内联函数