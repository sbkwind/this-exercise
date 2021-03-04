```js
function Foo() {
  this.a = () => console.log(this)
}

let foo = new Foo()
foo.a() // foo
```

```js
class Foo {
  constructor() {
    this.a = () => console.log(this)
  }
}

let foo = new Foo()
foo.a() // foo
```

```js
function Foo() {
  this.a = (function() {
    return () => console.log(this)
  })()
}

let foo = new Foo()
foo.a() // window
```

```js
class Foo {
  // 实例属性的简写
  a = (function() {
    return () => console.log(this)
  })()
}

let foo = new Foo()
foo.a() // undefined  class内为严格模式
```

```js
function Foo() {
  this.a = (() => () => console.log(this))()
}

let foo = new Foo()
foo.a() // foo
```

```js
class Foo {
  // 实例属性的简写，相当于在constructor内执行
  a = (() => () => console.log(this))()
}

let foo = new Foo()
foo.a() // foo
```

```js
function Foo() {}
Foo.prototype.a = () => console.log(this)

let foo = new Foo()
foo.a() // window
```

```js
class Foo {
  a() {
    return () => console.log(this)
  }
}

let foo = new Foo()
let log1 = foo.a
let log2 = foo.a()

log1()() // undefined
log2() // foo
```

```js
function Foo() {
  getName = function () {
    console.log(1)
  }
  return this
}
Foo.getName = function () {
  console.log(2)
}
Foo.prototype.getName = function () {
  console.log(3)
}
var getName = function () {
  console.log(4)
}
function getName() {
  console.log(5)
}

Foo.getName() // 2
getName() // 4
Foo().getName() // 1
getName() // 1
new Foo.getName() // 2
// 关于运算符优先级的问题，new和new右边最近的()配对即可
// let ins = new Foo()
// ins.getName()
new Foo().getName() // 3
// let ins = new Foo()
// new ins.getName()
new new Foo().getName() // 3
```

```js
function logLen() {
  console.log(this.length)
}
let obj = {
  length: 5,
  log(fn) {
    arguments[0]()
  }
}
obj.log(logLen) // 1   this -> arguments，因此输出了arguments.length
```

```js
var name = 'window'

let obj1 = {
  name: 'obj1',
  fn1: function () {
    console.log(this.name)
  },
  fn2: () => console.log(this.name),
  fn3: function () {
    return function () {
      console.log(this.name)
    }
  },
  fn4: function () {
    return () => console.log(this.name)
  }
}

let obj2 = {
  name: 'obj2'
}

obj1.fn1() // obj1
obj1.fn1.call(obj2) // obj2

obj1.fn2() // window
obj1.fn2.call(obj2) // window

obj1.fn3()() // window
obj1.fn3().call(obj2) // obj2
obj1.fn3.call(obj2)() // window

obj1.fn4()() // obj1
obj1.fn4().call(obj2) // obj1
obj1.fn4.call(obj2)() // obj2
```





















