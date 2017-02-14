
### this, call, apply, bind.
* this

Указатель на объект. В JS происходит позднее связывание и this определяется при вызове функции
```javascript
unction foo() {
    console.log( this.bar );
}

var bar = "global";

var obj1 = {
    bar: "obj1",
    foo: foo
};

var obj2 = {
    bar: "obj2"
};

// --------

foo();              // "global"
obj1.foo();         // "obj1"
foo.call( obj2 );   // "obj2"
new foo();          // undefined
```

---

### prototype, рассказ что это и как работает.

---
### ООП в js: наследование, инкапсуляция


## JavaScript Фронтэнд

### способы оптимизации работы с ДОМом. Список самых дорогих операций

---

### манипуляции с DOM - поиск, добавление, изменение, удаление классов и узлов

---


## JavaScript c7s

### понимание и обоснование каждого из пунктов google javascript styleguide. Это 80% по JS.
