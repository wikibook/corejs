### 6-1

```js
var Person = function(name) {
  this._name = name;
};
Person.prototype.getName = function() {
  return this._name;
};
```

### 6-2

```js
var Constructor = function(name) {
  this.name = name;
};
Constructor.prototype.method1 = function() {};
Constructor.prototype.property1 = 'Constructor Prototype Property';

var instance = new Constructor('Instance');
console.dir(Constructor);
console.dir(instance);
```

### 6-3

```js
var arr = [1, 2];
Array.prototype.constructor === Array; // true
arr.__proto__.constructor === Array; // true
arr.constructor === Array; // true

var arr2 = new arr.constructor(3, 4);
console.log(arr2); // [3, 4]
```

### 6-4

```js
var NewConstructor = function() {
  console.log('this is new constuctor!');
};
var dataTypes = [
  1, // Number & false
  'test', // String & false
  true, // Boolean & false
  {}, // NewConstructor & false
  [], // NewConstructor & false
  function() {}, // NewConstructor & false
  /test/, // NewConstructor & false
  new Number(), // NewConstructor & false
  new String(), // NewConstructor & false
  new Boolean(), // NewConstructor & false
  new Object(), // NewConstructor & false
  new Array(), // NewConstructor & false
  new Function(), // NewConstructor & false
  new RegExp(), // NewConstructor & false
  new Date(), // NewConstructor & false
  new Error(), // NewConstructor & false
];

dataTypes.forEach(function(d) {
  d.constructor = NewConstructor;
  console.log(d.constructor.name, '&', d instanceof NewConstructor);
});
```

### 6-5

```js
var Person = function(name) {
  this.name = name;
};
var p1 = new Person('사람1'); // Person { name: "사람1" } true
var p1Proto = Object.getPrototypeOf(p1);
var p2 = new Person.prototype.constructor('사람2'); // Person { name: "사람2" } true
var p3 = new p1Proto.constructor('사람3'); // Person { name: "사람3" } true
var p4 = new p1.__proto__.constructor('사람4'); // Person { name: "사람4" } true
var p5 = new p1.constructor('사람5'); // Person { name: "사람5" } true

[p1, p2, p3, p4, p5].forEach(function(p) {
  console.log(p, p instanceof Person);
});
```

### 6-6

```js
var Person = function(name) {
  this.name = name;
};
Person.prototype.getName = function() {
  return this.name;
};

var iu = new Person('지금');
iu.getName = function() {
  return '바로 ' + this.name;
};
console.log(iu.getName()); // 바로 지금
```

### 6-7

```js
var arr = [1, 2];
arr.push(3);
arr.hasOwnProperty(2); // true
```

### 6-8

```js
var arr = [1, 2];
Array.prototype.toString.call(arr); // 1,2
Object.prototype.toString.call(arr); // [object Array]
arr.toString(); // 1,2

arr.toString = function() {
  return this.join('_');
};
arr.toString(); // 1_2
```

### 6-9

```js
Object.prototype.getEntries = function() {
  var res = [];
  for (var prop in this) {
    if (this.hasOwnProperty(prop)) {
      res.push([prop, this[prop]]);
    }
  }
  return res;
};
var data = [
  ['object', { a: 1, b: 2, c: 3 }], // [["a",1], ["b", 2], ["c",3]]
  ['number', 345], // []
  ['string', 'abc'], // [["0","a"], ["1","b"], ["2","c"]]
  ['boolean', false], // []
  ['func', function() {}], // []
  ['array', [1, 2, 3]], // [["0", 1], ["1", 2], ["2", 3]]
];
data.forEach(function(datum) {
  console.log(datum[1].getEntries());
});
```

### 6-10

```js
var Grade = function() {
  var args = Array.prototype.slice.call(arguments);
  for (var i = 0; i < args.length; i++) {
    this[i] = args[i];
  }
  this.length = args.length;
};
var g = new Grade(100, 80);
```
