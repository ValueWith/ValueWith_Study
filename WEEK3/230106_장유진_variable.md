# 변수 선언(var, let, const)

변수란(variable)는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체, 또는 그 메모리 공간을 식별하기 위해 붙인 이름을 말합니다. 이 변수를 사용하기 위해서는 반드시 선언을 해줘야 하는데, 이 때 사용되는 키워드가 var, let, const입니다. var와 let은 변수를, const는 상수를 선언할 때 사용됩니다.

let과 const는 ES6에서 도입된 문법인데요. 그렇다면 var와 let, const는 어떤 차이를 가지고 있을까요?

# 스코프(Scope)

**스코프(Scope)란 코드가 각 상수나 변수에 접근할 수 있는 범위를 뜻합니다.**

스코프의 종류는 전역 스코프(Global Scope)와 지역 스코프(Local Scope) 두 가지로

지역 스코프는 다시 함수 스코프(Function Scope), 블록 스코프(Block Scope)로 구분이 됩니다.

**var는 함수 스코프를, const와 let은 블록 스코프를 가지고 있습니다.**

## 함수 스코프(Function Scope)

var로 변수를 선언하게 될 경우, 자동으로 함수 스코프를 가지게 됩니다.

변수가 함수 스코프를 가지고 있다는 것은 **변수가 선언된 함수 내부에서만 접근이 가능하다는 것을 의미**합니다.

```jsx
function main() {
  var x = 'hi';
}

console.log(x); // Reference Error
```

함수 스코프를 가지고 있기 때문에, 블록에 선언되었더라도 해당 함수 안이라면 어디서든지 접근이 가능합니다.

```jsx
function main() {
  if (true) {
    var x = 'hi';
    console.log(x); // 'hi'
  }
  console.log(x); // 'hi'
}

console.log(x); // Reference Error
```

## 블록 스코프(Block Scope)

let 과 const 키워드를 사용해 선언된 변수는 블록 스코프를 가지게 됩니다.

변수가 블록 스코프를 가지고 있다는 것은 **변수가 선언된 코드 블록 안에서만 접근이 가능하다는 것을 의미**합니다.

```jsx
function main() {
  if (true) {
    let x = 'hi';
    console.log(x); // 'hi'
  }
  console.log(x); // Reference Error
}

console.log(x); // Reference Error
```

## 전역 스코프(Global Scope)

만약 변수가 블록이나 함수 바깥에 선언되어 있다면, 전역 스코프를 가진 전역 변수가 됩니다.

블록 내부, 함수 내부 어디에서든지 참조가 가능합니다.

## 번외. 렉시컬 스코프(**Lexical Scope)**

렉시컬 스코핑이란 '정적 스코핑(Static Scoping)'이라고도 불립니다. **함수를 호출한 곳이 아닌 선언한 곳을 기준으로 스코프를 결정**하는 원칙의 이름입니다.

렉시컬 스코프란 특정한 스코프를 지칭하는 것이 아니라 전역 스코프, 함수 스코프, 블록 스코프에서 ‘렉시컬 스코핑 원칙을 따른 범위’라고 할 수 있습니다.

```jsx
var x = 'global';

function outer() {
  var x = 'outer';

  function inner() {
    var x = 'inner';
    console.log(x);
  }

  inner();
}

outer(); // 결과: 'inner'
console.log(x); // 결과: 'global'
```

`inner` 함수는 자신의 렉시컬 스코프에서 `x`라는 변수를 찾습니다. 이 경우, `inner` 함수 내부에 `x`라는 변수가 선언되어 있으므로 이를 출력하게 됩니다. 그런데 만약 `inner` 함수 내부에 `x` 변수의 선언이 없다면 어떻게 될까요?

```jsx
var x = 'global';

function outer() {
  var x = 'outer';

  function inner() {
    console.log(x);
  }

  inner();
}

outer(); // 결과: 'outer'
console.log(x); // 결과: 'global'
```

`inner` 함수는 자신의 렉시컬 스코프에서 `x`라는 변수를 찾습니다. 하지만 이번에는 `inner` 함수 내부에 `x` 변수의 선언이 없습니다. 그래서 상위 스코프인 `outer` 함수의 스코프로 이동하여 `x` 변수를 찾게 되고, 이 `x`를 출력하게 됩니다. 이러한 과정을 **스코프 체인(Scope Chain)**이라고 합니다.

### ❗️스코프 체인(Scope Chain)

스코프 체인이란 코드 블록 내에서 변수를 찾는 과정을 말합니다. 코드는 먼저 자신이 위치한 스코프에서 변수를 찾습니다. 만약 그 변수가 해당 스코프내에 없다면, 이 코드는 상위 레벨의 스코프으로 이동하여 변수를 찾습니다. 그리고 이렇게 계속해서 변수를 찾을 때까지 상위 레벨의 스코프로 이동하는 과정을 스코프 체인이라고 합니다.

# 중복 선언(Variable Redeclaration)

var와 let은 둘 다 변수를 선언하는 키워드입니다.

**var 의 경우 같은 스코프 내에서 중복 선언을 허용**하고, let은 중복 선언을 허용하지 않는다는 특징이 있습니다.

같은 이름으로 선언했다면, 나중에 선언한 값으로 원래의 값이 덮어씌워집니다.

```jsx
var x = '안녕하세요'

...

var x = 'hello'

console.log(x) // 'hello'
```

```jsx
let x = '안녕하세요'

...

let x = 'hello'

console.log(x) // SystexError : Identifier 'x' has already been declared
```

# 호이스팅(Hoisting)

자바스크립트는 변수를 사용하기 위해서 3가지 단계를 거치게 됩니다.

1. 선언 단계 - 자바스크립트 엔진에게 변수의 이름을 등록해서 알리는 단계
2. 초기화 단계 - 값을 저장하기 위한 메모리 공간을 확보하고 초기화하는 단계
3. 할당 단계 - 할당된 메모리 공간에 값을 저장하는 단계

호이스팅이란 변수와 함수 선언을 해당 스코프의 최상단으로 끌어올리는 것을 말합니다.

다음 코드를 한 번 볼까요?

```jsx
console.log(num); // undefined

var num = 10;

console.log(num); // 10
```

```jsx
foo(); // "hello"
bar(); // undefined

function foo() {
  // 함수선언문
  console.log('hello');
}

var bar = function () {
  // 함수표현식
  console.log('hello2');
};
```

var는 선언과 초기화가 동시에 이루어집니다. 선언을 최상단으로 끌어올리면서 암묵적으로 undefined를 할당합니다. 때문에 var 변수들은 선언 이전에 접근하더라도 에러를 발생시키지 않습니다.

실질적으로 다음과 같은 코드입니다.

```jsx
var num; // 여기에서, undefined 으로 초기화

console.log(num); // undefined

num = 10;

console.log(num); // 10
```

let은 어떻게 될까요? 같은 경우에서 참조 오류(Reference Error)를 발생시키게 됩니다.

let도 호이스팅이 일어납니다. 그렇다면 왜 이런 에러가 발생할까요?

```jsx
console.log(num); // Reference Error : Cannot access 'num' before initialization

let num = 10;

console.log(num); // 10
```

var와 다르게 let은 선언을 최상단으로 끌어올릴 뿐, 초기화를 하지 않기 때문입니다.

let은 선언된 변수의 값에 값이 할당되기 전까지는 **TDZ(Temporal Dead Zone)**에 들어가게 됩니다.

자바스크립트는 TDZ에 있는 변수에 접근을 허용하지 않기 때문에 참조 오류를 발생시키게 됩니다.

❗️참조 오류(Reference Error) : 식별자를 통해 값을 참조하려 했지만 자바스크립트 엔진이 등록된 식별자(변수 이름)을 찾을 수 없을 때 발생하는 에러

```jsx
var x = 10;
function someFunction() {
  console.log(x); // 결과: undefined
  var x = 20;
}
someFunction();
```

# let과 const를 사용하자

여기까지 var키워드와 let(const)의 차이, 스코프 / 중복 선언 / 호이스팅 세 가지에 대해 살펴보았습니다.

각 차이를 살펴보면서 var 키워드를 사용하는 경우 코드가 의도한 것과 다르게 동작할 수 있다는 것을 알 수 있었습니다. 짧은 코드라면 괜찮겠지만 다른 사람들과 협업해야 하는 상황에서 혹은 코드가 길어져서 이미 선언한 식별자 이름을 기억하지 못하고 재선언한다면? 상수로 쓸 값을 덮어씌워버린다면?

let과 const는 var에 비해 더 안정적이고 예측 가능한 코드를 작성할 수 있게 도와줍니다. 그래서 코드를 작성할 때는 var보다는 let이나 const를 사용하는 것을 권장하고 있습니다.
