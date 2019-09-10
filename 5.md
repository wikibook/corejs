### 5-1

```js
var outer = function() {
  var a = 1;
  var inner = function() {
    console.log(++a);
  };
  inner();
};
outer();
```

### 5-2

```js
var outer = function() {
  var a = 1;
  var inner = function() {
    return ++a;
  };
  return inner();
};
var outer2 = outer();
console.log(outer2); // 2
```

### 5-3

```js
var outer = function() {
  var a = 1;
  var inner = function() {
    return ++a;
  };
  return inner;
};
var outer2 = outer();
console.log(outer2()); // 2
console.log(outer2()); // 3
```

### 5-4

```js
// (1) setInterval/setTimeout
(function() {
  var a = 0;
  var intervalId = null;
  var inner = function() {
    if (++a >= 10) {
      clearInterval(intervalId);
    }
    console.log(a);
  };
  intervalId = setInterval(inner, 1000);
})();
```

```js
// (2) eventListener
(function() {
  var count = 0;
  var button = document.createElement('button');
  button.innerText = 'click';
  button.addEventListener('click', function() {
    console.log(++count, 'times clicked');
  });
  document.body.appendChild(button);
})();
```

### 5-5

```js
// (1) return에 의한 클로저의 메모리 해제
var outer = (function() {
  var a = 1;
  var inner = function() {
    return ++a;
  };
  return inner;
})();
console.log(outer());
console.log(outer());
outer = null; // outer 식별자의 inner 함수 참조를 끊음
```

```js
// (2) setInterval에 의한 클로저의 메모리 해제
(function() {
  var a = 0;
  var intervalId = null;
  var inner = function() {
    if (++a >= 10) {
      clearInterval(intervalId);
      inner = null; // inner 식별자의 함수 참조를 끊음
    }
    console.log(a);
  };
  intervalId = setInterval(inner, 1000);
})();
```

```js
// (3) eventListener에 의한 클로저의 메모리 해제
(function() {
  var count = 0;
  var button = document.createElement('button');
  button.innerText = 'click';

  var clickHandler = function() {
    console.log(++count, 'times clicked');
    if (count >= 10) {
      button.removeEventListener('click', clickHandler);
      clickHandler = null; // clickHandler 식별자의 함수 참조를 끊음
    }
  };
  button.addEventListener('click', clickHandler);
  document.body.appendChild(button);
})();
```

### 5-6

```js
var fruits = ['apple', 'banana', 'peach'];
var $ul = document.createElement('ul'); // (공통 코드)

fruits.forEach(function(fruit) {
  // (A)
  var $li = document.createElement('li');
  $li.innerText = fruit;
  $li.addEventListener('click', function() {
    // (B)
    alert('your choice is ' + fruit);
  });
  $ul.appendChild($li);
});
document.body.appendChild($ul);
```

### 5-7

```js
var fruits = ['apple', 'banana', 'peach'];
var $ul = document.createElement('ul');

var alertFruit = function(fruit) {
  alert('your choice is ' + fruit);
};
fruits.forEach(function(fruit) {
  var $li = document.createElement('li');
  $li.innerText = fruit;
  $li.addEventListener('click', alertFruit);
  $ul.appendChild($li);
});
document.body.appendChild($ul);
alertFruit(fruits[1]);
```

### 5-8

```js
var fruits = ['apple', 'banana', 'peach'];
var $ul = document.createElement('ul');

var alertFruit = function(fruit) {
  alert('your choice is ' + fruit);
};
fruits.forEach(function(fruit) {
  var $li = document.createElement('li');
  $li.innerText = fruit;
  $li.addEventListener('click', alertFruit.bind(null, fruit));
  $ul.appendChild($li);
});
document.body.appendChild($ul);
```

### 5-9

```js
var fruits = ['apple', 'banana', 'peach'];
var $ul = document.createElement('ul');

var alertFruitBuilder = function(fruit) {
  return function() {
    alert('your choice is ' + fruit);
  };
};
fruits.forEach(function(fruit) {
  var $li = document.createElement('li');
  $li.innerText = fruit;
  $li.addEventListener('click', alertFruitBuilder(fruit));
  $ul.appendChild($li);
});
document.body.appendChild($ul);
```

### 5-10

```js
var car = {
  fuel: Math.ceil(Math.random() * 10 + 10), // 연료(L)
  power: Math.ceil(Math.random() * 3 + 2), // 연비(km/L)
  moved: 0, // 총 이동거리
  run: function() {
    var km = Math.ceil(Math.random() * 6);
    var wasteFuel = km / this.power;
    if (this.fuel < wasteFuel) {
      console.log('이동불가');
      return;
    }
    this.fuel -= wasteFuel;
    this.moved += km;
    console.log(km + 'km 이동 (총 ' + this.moved + 'km)');
  },
};
```

### 5-11

```js
var createCar = function() {
  var fuel = Math.ceil(Math.random() * 10 + 10); // 연료(L)
  var power = Math.ceil(Math.random() * 3 + 2); // 연비(km / L)
  var moved = 0; // 총 이동거리
  return {
    get moved() {
      return moved;
    },
    run: function() {
      var km = Math.ceil(Math.random() * 6);
      var wasteFuel = km / power;
      if (fuel < wasteFuel) {
        console.log('이동불가');
        return;
      }
      fuel -= wasteFuel;
      moved += km;
      console.log(km + 'km 이동 (총 ' + moved + 'km). 남은 연료: ' + fuel);
    },
  };
};
var car = createCar();
```

### 5-12

```js
var createCar = function() {
  var fuel = Math.ceil(Math.random() * 10 + 10); // 연료(L)
  var power = Math.ceil(Math.random() * 3 + 2); // 연비(km / L)
  var moved = 0; // 총 이동거리
  var publicMembers = {
    get moved() {
      return moved;
    },
    run: function() {
      var km = Math.ceil(Math.random() * 6);
      var wasteFuel = km / power;
      if (fuel < wasteFuel) {
        console.log('이동불가');
        return;
      }
      fuel -= wasteFuel;
      moved += km;
      console.log(km + 'km 이동 (총 ' + moved + 'km). 남은 연료: ' + fuel);
    },
  };
  Object.freeze(publicMembers);
  return publicMembers;
};
var car = createCar();
```

### 5-13

```js
var add = function() {
  var result = 0;
  for (var i = 0; i < arguments.length; i++) {
    result += arguments[i];
  }
  return result;
};
var addPartial = add.bind(null, 1, 2, 3, 4, 5);
console.log(addPartial(6, 7, 8, 9, 10)); // 55
```

### 5-14

```js
var partial = function() {
  var originalPartialArgs = arguments;
  var func = originalPartialArgs[0];
  if (typeof func !== 'function') {
    throw new Error('첫 번째 인자가 함수가 아닙니다.');
  }
  return function() {
    var partialArgs = Array.prototype.slice.call(originalPartialArgs, 1);
    var restArgs = Array.prototype.slice.call(arguments);
    return func.apply(this, partialArgs.concat(restArgs));
  };
};

var add = function() {
  var result = 0;
  for (var i = 0; i < arguments.length; i++) {
    result += arguments[i];
  }
  return result;
};
var addPartial = partial(add, 1, 2, 3, 4, 5);
console.log(addPartial(6, 7, 8, 9, 10)); // 55

var dog = {
  name: '강아지',
  greet: partial(function(prefix, suffix) {
    return prefix + this.name + suffix;
  }, '왈왈, '),
};
dog.greet('입니다!'); // 왈왈, 강아지입니다.
```

### 5-15

```js
Object.defineProperty(window, '_', {
  value: 'EMPTY_SPACE',
  writable: false,
  configurable: false,
  enumerable: false,
});

var partial2 = function() {
  var originalPartialArgs = arguments;
  var func = originalPartialArgs[0];
  if (typeof func !== 'function') {
    throw new Error('첫 번째 인자가 함수가 아닙니다.');
  }
  return function() {
    var partialArgs = Array.prototype.slice.call(originalPartialArgs, 1);
    var restArgs = Array.prototype.slice.call(arguments);
    for (var i = 0; i < partialArgs.length; i++) {
      if (partialArgs[i] === _) {
        partialArgs[i] = restArgs.shift();
      }
    }
    return func.apply(this, partialArgs.concat(restArgs));
  };
};

var add = function() {
  var result = 0;
  for (var i = 0; i < arguments.length; i++) {
    result += arguments[i];
  }
  return result;
};
var addPartial = partial2(add, 1, 2, _, 4, 5, _, _, 8, 9);
console.log(addPartial(3, 6, 7, 10)); // 55

var dog = {
  name: '강아지',
  greet: partial2(function(prefix, suffix) {
    return prefix + this.name + suffix;
  }, '왈왈, '),
};
dog.greet(' 배고파요!'); // 왈왈, 강아지 배고파요!
```

### 5-16

```js
var debounce = function(eventName, func, wait) {
  var timeoutId = null;
  return function(event) {
    var self = this;
    console.log(eventName, 'event 발생');
    clearTimeout(timeoutId);
    timeoutId = setTimeout(func.bind(self, event), wait);
  };
};

var moveHandler = function(e) {
  console.log('move event 처리');
};
var wheelHandler = function(e) {
  console.log('wheel event 처리');
};
document.body.addEventListener('mousemove', debounce('move', moveHandler, 500));
document.body.addEventListener(
  'mousewheel',
  debounce('wheel', wheelHandler, 700)
);
```

### 5-17

```js
var curry3 = function(func) {
  return function(a) {
    return function(b) {
      return func(a, b);
    };
  };
};

var getMaxWith10 = curry3(Math.max)(10);
console.log(getMaxWith10(8)); // 10
console.log(getMaxWith10(25)); // 25

var getMinWith10 = curry3(Math.min)(10);
console.log(getMinWith10(8)); // 8
console.log(getMinWith10(25)); // 10
```

### 5-18

```js
var curry5 = function(func) {
  return function(a) {
    return function(b) {
      return function(c) {
        return function(d) {
          return function(e) {
            return func(a, b, c, d, e);
          };
        };
      };
    };
  };
};
var getMax = curry5(Math.max);
console.log(getMax(1)(2)(3)(4)(5));
```
