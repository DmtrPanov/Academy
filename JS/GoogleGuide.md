#### var
Всегда объявлять переменные с `var`  
Если ключевое слово `var` отсутствует, то переменная определяется как глобальная. Также возможны затруднения при определения окружения в котором переменная используется.

#### Constants
* NAME_LIKE_THIS
* Использовать `@const`
* Не использовать `const`, поскольку это ключевое слово не поддерживается Internet Explorer  

Свойства константы также будут являться константами.  
Примитивные типы (`number`, `string`, `boolean`) - константы  
Объекты должны быть неизменны, если они не зависят от изменения состояния. Это не отслеживается при компиляции.  
`@const` подразумевает, что:  
 - переменная или свойство не будет переопределяться (изменять значение). Это проверяется во время компиляции. (работает аналогично объявлению с ключивым словом `const`).  
- Методы не могут быть переопределены в подклассах
- Классы не могут иметь подклассов

*Примеры*
```javascript
/**
 * Request timeout in milliseconds.
 * @type {number}
 */
goog.example.TIMEOUT_IN_MILLISECONDS = 60;
```
Количество секунд в минуте никогда не меняется. `ALL_CAPS` также будут константами.

```javascript
/**
 * Map of URL to response string.
 * @const
 */
MyClass.fetchedUrlCache_ = new goog.structs.Map();
/**
 * Class that cannot be subclassed.
 * @const
 * @constructor
 */
sloth.MyFinalClass = function() {};
```

#### Semicolons

Всегда использовать `;`
`;` должна завершать выражение функции, но не ее объявление.
```javascript
var foo = function() {
  return true;
};  // semicolon here.

function foo() {
  return true;
}  // no semicolon here.
```

#### Function Declarations Within Blocks

Не надо делать так:
```javascript
if (x) {
  function foo() {}
}
```
ECMAScript позволяет написать такой код, но это не является стандартом. Следует объявить новую переменную.
```javascript
if (x) {
  var foo = function() {};
}
```

#### Wrapper objects for primitive types

Нет причин для обертки примитивных типов в объекты. К тому же это может привести к ошибкам:
```javascript
var x = new Boolean(false);
if (x) {
  alert('hi');  // Shows 'hi'.
}
```
Следует поступать следующим образом:
```javascript
var x = Boolean(0);
if (x) {
  alert('hi');  // This will never be alerted.
}
typeof Boolean(0) == 'boolean';
typeof new Boolean(0) == 'object';
```

#### Multi-level prototype hierarchies

Потомок может иметь только одного родителя.
Но если все же нужно реализовать множественное наследование, следует использовать `goog.inherits()`
```javascript
function D() {
  goog.base(this)
}
goog.inherits(D, B);

D.prototype.method = function() {
  ...
};
```

#### Method and property definitions
Примеры объявления методов и свойств для объекта, созданного с помощью `new`
```javascript
Foo.prototype.bar = function() {
  /* ... */
};
```
```javascript
/** @constructor */
function Foo() {
  this.bar = value;
}
```
