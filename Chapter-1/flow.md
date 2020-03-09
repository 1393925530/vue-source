#Flow
##认识Flow
Flow是一个JavaScript静态类型检查工具。Vue.js的源码利用了Flow做了静态类型检查。
##为什么用Flow
JavaScript是灵活的动态类型语言，但是灵活性的副作用是容易写出隐蔽的隐患代码，编译期不会报错，运行阶段出现BUG。
类型检查就是在编译期尽早发现由类型错误引起的BUG，又不影响代码运行，使编写JavaScript具有和编写Java等强类型语言相近的体验。
之所以Vue.js 2.0重构选择Flow，主要是因为Babel和Eslint有对应的Flow插件支持语法，可以完全沿用现有的构建配置，改动小的情况下拥有静态类型检查的能力。
##Flow的工作方式
通常类型检查分成2种方式：
- 类型推断：通关变量的使用上下文来推断出变量类型，然后根据这些推断来检查类型。
- 类型注释：事先注释好期待的类型，Flow会基于这些注释来判断。
###类型判断
不需要任何代码修改即可进行类型检查，最小化工作量。不会强制改变开发习惯，因为会自动推断变量的类型。例子说明：
```JavaScript
/*@flow*/
function split(str) {
  return str.split(' ')
}
split(11)
```
Flow检查上述代码后会报错，因为函数`split`期待的参数是字符串，输入了数字。
###类型注释
类型推断是Flow最有用的场景之一，不需要编写类型注释就能获得有用的反馈。但在某些场景下，添加类型注释可以提供更好更明确的检查依据。
考虑如下代码：
```JavaScript
/*@flow*/
function add(x, y) {
  return x + y
}
add('Hello', 11)
```
Flow检查上述代码检查不出错误，因为从语法层面考虑，`+`既可以用在字符串上，也可以用在数字上，没有明确指出`add()`的参数必须是数字。
在这种情况下，可以借助类型注释来指明期待的类型。类型注释是以冒号`:`开头，可以在函数参数、返回值、变量声明中使用。
如果在上段代码中添加类型注释，就会变成如下：
```JavaScript
/*@flow*/
function add(x: number, y: number): number {
  return x + y
}
add('Hello', 11)
```
现在Flow就能检查出错误，因为函数参数的期待类型为数字，而提供的是字符串。
上面的例子是针对函数的类型注释。接下来看看Flow能支持的一些常见的类型注释。
- 数组
```JavaScript
/*@flow*/
var arr: Array<number> = [1, 2, 3]
arr.push('Hello')
```
数组类型注释的格式是`Array<T>`，`T`表示数组中每项的数据类型。上述代码arr是每项均为数字的数组。给数组添加字符串，Flow能检查出错误。
- 类和对象
```JavaScript
/*@flow*/
class Bar {
  x: string; // x是字符串
  y: string | number; // y可以是字符串或数字
  z: boolean;
  constructor(x: string, y: string | number) {
    this.x = x
    this.y = y
    this.z = false
  }
}
var bar: Bar = new Bar('hello', 4) 
var obj: { a: string, b: number, c: Array<string>, d: Bar } = {
  a: 'hello',
  b: 11,
  c: ['hello', 'world'],
  d: new Bar('hello', 3)
}
```
类的注释类型格式如上，可以对类自身的属性做类型检查，也可以对构造函数的参数做类型检查。需要注意的是，属性`y`的类型中间用`|`做间隔，表示`y`的类型既可以是字符串也可以是数字。
对象的注释类似于类，需要指定对象属性的类型。
- Null
若想任意类型`T`可以为`null`或`undefined`，只需类似如下写成`?T`的格式即可。
```JavaScript
/*@flow*/
var foo: ?string = null
```
此时，`foo`既可以为字符串，也可以为`null`。
###Flow在Vue.js源码中的应用
为了解决引用第三方库或自定义类型检查报错问题，Flow提出了一个`libref`的概念，可用来识别第三方库或者是自定义类型，Vue.js也利用了这个特性。
在Vue.js主目录下有`.flowconfig`文件，其中的`[libs]`部分用来描述包含指定库定义的目录，默认是名为`flow-typed`的目录。
这里`[libs]`配置的是`flow`，表示指定的库定义都在`flow`文件夹内。
#总结
类似于Flow这种静态类型检查的方式非常有利于大型项目源码的开发和维护。
