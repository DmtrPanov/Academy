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

#### delete

Предпочтительней использовать присваивание значения `null`
```javascript
Foo.prototype.dispose = function() {
  this.property_ = null;
};
```
Изменение количества свойст работает горазда медленней, чем присваивание нового значения.

#### Closures

Можно использовать замыкания, но нужно иметь ввиду, что они могут привести к утечки памяти. Поскольку замыкание выделяет память под замыкаемые элементы, даже если они не будут использоваться.
```javascript
function foo(element, a, b) {
  element.onclick = function() { /* uses a and b */ };
}
```
Ссылка на `element`, `a`, `b` будет храниться, даже если `element` не будет использоваться.
В данном примере следует использовать следующую структуру:
```javascript
function foo(element, a, b) {
  element.onclick = bar(a, b);
}

function bar(a, b) {
  return function() { /* uses a and b */ };
}
```

#### with() {}

Не следует использовать. Может привести к непредсказуемым последствиям при пересечении свойств и локальных переменных.

#### for-in loop

Только для прохождения по ключам объекта. Но и для этих целей лучше использовать другие методы.


#### Associative Arrays

Никогда не используйте `Array` как ассоциативный массив. Используйте объекты.
Массивы не позволят использовать никакой индексации кроме нумерологической.

JavaScript Style Rules
===
#### Naming

```
functionNamesLikeThis
variableNamesLikeThis
ClassNamesLikeThis
EnumNamesLikeThis
methodNamesLikeThis
CONSTANT_VALUES_LIKE_THIS
foo.namespaceNamesLikeThis.bar
filenameslikethis.js.
_privateProperties
protectedProperties 
```



```javascript
```
