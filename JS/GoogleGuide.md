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

#### Visibility (private and protected fields)

Можно и нужно. Для описания использовать JSDoc.  
Таблица популярных тегов
Тег |	Описание
:---|:---
@author |	Имя разработчика
@constructor |	Маркирует функцию как конструктор
@deprecated |	Маркирует метод устаревшим и не рекомендуемым
@exception |	Синоним для @throws
@param |	Описывает аргумент функции; можно указать тип, задав его в фигурных скобках
@private |	Означает, что метод приватный
@return |	Описывает возвращаемое значение
@returns |	Синоним return
@see |	Описывает связь с другим объектом
@this |	Задает тип объекта, на который указывает ключевое слово «this» внутри функции.
@throws |	Описывает исключения, выбрасываемые методом
@version |	Версия библиотеки

`@private` доступны в рамках файла, методам и свойствам, если свойства принадлежат классу. Не может быть переопределен в подклассах в других файлах.

`@protected` доступны в рамках файла и подклассам
```javascript
// File 1.

/** @constructor */
AA_PublicClass = function() {
  /** @private */
  this.privateProp_ = 2;

  /** @protected */
  this.protectedProp = 4;
};

/** @private */
AA_PublicClass.staticPrivateProp_ = 1;

/** @protected */
AA_PublicClass.staticProtectedProp = 31;

/** @private */
AA_PublicClass.prototype.privateMethod_ = function() {};

/** @protected */
AA_PublicClass.prototype.protectedMethod = function() {};

// File 2.

/**
 * @return {number} The number of ducks we've arranged in a row.
 */
AA_PublicClass.prototype.method = function() {
  // Legal accesses of these two properties.
  return this.privateProp_ + AA_PublicClass.staticPrivateProp_;
};

// File 3.

/**
 * @constructor
 * @extends {AA_PublicClass}
 */
AA_SubClass = function() {
  // Legal access of a protected static property.
  AA_PublicClass.staticProtectedProp = this.method();
};
goog.inherits(AA_SubClass, AA_PublicClass);

/**
 * @return {number} The number of ducks we've arranged in a row.
 */
AA_SubClass.prototype.method = function() {
  // Legal access of a protected instance property.
  return this.protectedProp;
};
```

#### JavaScript Types


Syntax  Name |	Syntax	| Description |	Deprecated Syntaxes
---|---|---|---
Primitive Type | There are 5 primitive types in JavaScript: {null}, {undefined}, {boolean}, {number}, and {string}.| Simply the name of a type.|
Instance Type| {Object} An instance of Object or null. {Function} An instance of Function or null. {EventTarget} An instance of a constructor that implements the EventTarget interface, or null. | An instance of a constructor or interface function. Constructor functions are functions defined with the @constructor JSDoc tag. Interface functions are functions defined with the @interface JSDoc tag. By default, instance types will accept null. This is the only type syntax that makes the type nullable. Other type syntaxes in this table will not accept null. |
Enum Type | {goog.events.EventType} One of the properties of the object literal initializer of goog.events.EventType. | An enum must be initialized as an object literal, or as an alias of another enum, annotated with the @enum JSDoc tag. The properties of this literal are the instances of the enum. The syntax of the enum is defined below. Note that this is one of the few things in our type system that were not in the ES4 spec. |
Type Application | {Array.<string>} An array of strings. {Object.<string, number>}  An object in which the keys are strings and the values are numbers. | Parameterizes a type, by applying a set of type arguments to that type. The idea is analogous to generics in Java. |
Type Union | `{(number l boolean)}` A number or a boolean. | Indicates that a value might have type A OR type B. The parentheses may be omitted at the top-level expression, but the parentheses should be included in sub-expressions to avoid ambiguity. `{number l boolean}` `{function(): (number l boolean)}` |` {(number,boolean)}, {(number ll boolean)}`
Nullable type | {?number} A number or null. | Shorthand for the union of the null type with any other type. This is just syntactic sugar. | {number?}
Non-nullable type | {!Object} An Object, but never the null value. |Filters null out of nullable types. Most often used with instance types, which are nullable by default. | {Object!}
Record Type | {{myNum: number, myObject}}  An anonymous type with the given type members. | Indicates that the value has the specified members with the specified types. In this case, myNum with a type number and myObject with any type. Notice that the braces are part of the type syntax. For example, to denote an Array of objects that have a length property, you might write Array.<{length}>. |
Function Type | {function(string, boolean)} A function that takes two arguments (a string and a boolean), and has an unknown return value. | Specifies a function. |
Function Return Type | {function(): number} A function that takes no arguments and returns a number. | Specifies a function return type. |
Function this Type | {function(this:goog.ui.Menu, string)} A function that takes one argument (a string), and executes in the context of a goog.ui.Menu. | Specifies the context type of a function type. |
Function new Type | {function(new:goog.ui.Menu, string)} A constructor that takes one argument (a string), and creates a new instance of goog.ui.Menu when called with the 'new' keyword. | Specifies the constructed type of a constructor. |
Variable arguments | {function(string, ...[number]): number} A function that takes one argument (a string), and then a variable number of arguments that must be numbers. | Specifies variable arguments to a function. |
Variable arguments (in @param annotations) | @param {...number} var_args A variable number of arguments to an annotated function. | Specifies that the annotated function accepts a variable number of arguments. |
Function optional arguments | {function(?string=, number=)} A function that takes one optional, nullable string and one optional number as arguments. The = syntax is only for function type declarations. | Specifies optional arguments to a function.|
Function optional arguments (in @param annotations) | @param {number=} opt_argument An optional parameter of type number. | Specifies that the annotated function accepts an optional argument. |
The ALL type | `{*}` |Indicates that the variable can take on any type. |
The UNKNOWN type | `{?}` |Indicates that the variable can take on any type, and the compiler should not type-check any uses of it.	|


Типы JavaScript
https://github.com/ivan-kleshnin/subjective-js-ru/blob/master/content/3.2.types-and-prototypes.md
Указание типа возможно в комментарии: `/** @type {number} */ (x)`  

Поскольку JavaScript является слаботипизированным языков, важно понять разницу между optional, nullable и undefined параметрами функций и свойств класса.  

Экземпляр класса и интерфейс по умолчанию nullable
```javascript
/**
 * Some class, initialized with a value.
 * @param {Object} value Some value.
 * @constructor
 */
function MyClass(value) {
  /**
   * Some value.
   * @type {Object}
   * @private
   */
  this.myValue_ = value;
}
```
В данном примере мы сообщаем компилятору, что свойство `myValue_` может содержать любой объект или null.

Если `myValue_` никогда не должно быть `null`, то следует изменить код следующим образом:
```javascript
/**
 * Some class, initialized with a non-null value.
 * @param {!Object} value Some value.
 * @constructor
 */
function MyClass(value) {
  /**
   * Some value.
   * @type {!Object}
   * @private
   */
  this.myValue_ = value;
}
```
Теперь если компилятор определит, что где-то в коде `myValue_` инициируется со значением `null`, то он выдаст предупреждение 'warning'  

Необязательные (optional) параметры для функции должны быть `undefined` в runtime.

```javascript
/**
 * Some class, initialized with an optional value.
 * @param {Object=} opt_value Some value (optional).
 * @constructor
 */
function MyClass(opt_value) {
  /**
   * Some value.
   * @type {Object|undefined}
   * @private
   */
  this.myValue_ = opt_value;
}
```
Мы сообщаем компилятору, что `myValue_` может содержать Object, null или остаться undefined  

Необязательный параметр `opt_value` объявляется как `{Object=}`, а не `{Object|undefined}`, поскольку он по определению может быть `undefined`.  

Еще один общий пример:
```javascript
/**
 * Takes four arguments, two of which are nullable, and two of which are
 * optional.
 * @param {!Object} nonNull Mandatory (must not be undefined), must not be null.
 * @param {Object} mayBeNull Mandatory (must not be undefined), may be null.
 * @param {!Object=} opt_nonNull Optional (may be undefined), but if present,
 *     must not be null!
 * @param {Object=} opt_mayBeNull Optional (may be undefined), may be null.
 */
function strangeButTrue(nonNull, mayBeNull, opt_nonNull, opt_mayBeNull) {
  // ...
};
```

Некоторые типы могут быть запутанными. В этом случае следует использовать `@typedof`
```javascript
/** @typedef {(string|Element|Text|Array.<Element>|Array.<Text>)} */
goog.ElementContent;

/**
 * @param {string} tagName
 * @param {goog.ElementContent} contents
 * @return {!Element}
 */
goog.createElement = function(tagName, contents) {
...
};
```

#### Comments

Для комментариев использовать JSDoc  
Все файлы, классы, методы и свойства должны сопровождаться комментарием JSDoc с соответствующим тегом или типом.  

**Поддержка HTML**
Комментарии JSDoc поддерживают HTML теги.  
```
/**
 * Computes weight based on three factors:
 * <ul>
 * <li>items sent
 * <li>items received
 * <li>last timestamp
 * </ul>
 */
```
**Информация о файле**
Информацию о файле рекомендуется добавлять, если в нем определяется более одного класса.
```
/**
 * @fileoverview Description of file, its uses and information
 * about its dependencies.
 */
```
**Классы**
Классы должны сопровождаться описанием, ключая описания типов.
```
/**
 * Class making something fun and easy.
 * @param {string} arg1 An argument that makes this more interesting.
 * @param {Array.<number>} arg2 List of numbers to be processed.
 * @constructor
 * @extends {goog.Disposable}
 */
project.MyClass = function(arg1, arg2) {
  // ...
};
goog.inherits(project.MyClass, goog.Disposable);
```

**Методы и функции**
Параметры и тип возвращаемого значения метода или функции следует документировать.
Метод можно не описывать, если его действия понятны исходя из описания параметров или возвращаемого значения.

```
/**
 * Operates on an instance of MyClass and returns something.
 * @param {project.MyClass} obj Instance of MyClass which leads to a long
 *     comment that needs to be wrapped to two lines.
 * @return {boolean} Whether something occurred.
 */
function PR_someMethod(obj) {
  // ...
}
```

**Свойства**
```
/** @constructor */
project.MyClass = function() {
  /**
   * Maximum number of things per pane.
   * @type {number}
   */
  this.someProperty = 4;
}
```


**Тег**  
`@author`  
**Образец и пример**  
`@author username@google.com (first last)`  
Пример  
```
/**
 * @fileoverview Utilities for handling textareas.
 * @author kuth@google.com (Uthur Pendragon)
 */
```
**Описание**  
Автор документа
---
**Тег**  
`@code`  
**Образец и пример**  
`{@code ...}``
Пример  
```
/**
 * Moves to the next position in the selection.
 * Throws {@code goog.iter.StopIteration} when it
 * passes the end of the range.
 * @return {Node} The node at the next position.
 */
goog.dom.RangeIterator.prototype.next = function() {
  // ...
};
```
**Описание**  
Указывает, что эта часть JSDoc код, поэтому ее корректнее форматировать ее при генерации документации  
---
**Тег**  

**Образец и пример**  

**Описание**
---


#### goog.provide
Все



```javascript
```
