# 동기(**Synchronous)와 비동기(Asynchronous)란?**

동기는 **직렬적(순차적)**으로 작동하는 방식입니다. 요청을 보내면 해당 요청이 완료된 후 다음 동작을 실행하는 방식입니다.

비동기는 **병렬적**으로 작동하는 방식입니다. 요청을 보내면 응답과 관계 없이 다음 동작을 실행하는 방식입니다.

# 자바스크립트의 비동기 처리

자바스크립트는 싱글 스레드로 돌아가는 언어이기 때문에 한 번에 한 작업만, 하나의 메인 스레드에서 처리될 수 있습니다. 다른 작업은 앞선 작업이 끝나야 수행됩니다. 즉, 자바스크립트는 **동기식 언어**입니다.

하지만 데이터 요청이나 타이머 등 오랜 시간이 걸리는 작업을 처리할 때에도 동기적으로 처리한다면 어떻게 될까요? 데이터가 불러와지는 동안 사용자는 버튼을 클릭하는 등 어떠한 액션을 취해도 아무 반응없는 화면을 마주해야 합니다.

이러한 문제를 해결하기 위해 자바스크립트는 비동기 처리를 지원합니다. 비동기 처리를 사용하면 데이터 요청이나 타이머와 같은 작업을 백그라운드에서 수행하고, 그 동안에도 다른 작업을 처리할 수 있습니다.
(비동기로 작동하는 코드들 → Ajax, EventListner, setTimeout 등)

하지만 비동기로 코드를 작성할 경우 코드의 동작을 예측하기 어렵고, 의도와 다르게 동작할 수 있다는 문제가 있습니다. 예시 코드를 하나 보겠습니다.

```jsx
console.log('1'); // 1

setTimeout(() => {
  console.log('2');
}, 2000);

console.log('3'); // 3
```

위 코드에서 기대하는 작동 방식은 console에 1, 2, 3이 순서대로 찍히는 것이지만, 실제로 console.log에서는 1, 3, 2의 순서대로 찍히게 됩니다.

setTimeout이 비동기로 동작하는 함수이기 때문에 브라우저(자바스크립트 엔진)가 setTimeout의 실행 결과를 기다리지 않고 바로 console.log(’3’)을 실행시키기 때문입니다.

이런 문제를 해결하기 위해, 즉 비동기 작업에서 순서를 보장하고 예측 가능한 동작을 위해 자바스크립트에서는 콜백 함수, Promise, async/await 세 가지 패턴이 사용됩니다.

## 콜백(Call back) 함수

다른 함수에 파라미터로 넘겨지는 함수를 콜백 함수라고 부릅니다.

자바스크립트에서 비동기 처리를 위해 가장 흔하게 사용되는 패턴으로 비동기 작업이 완료된 후에 호출되어 실행 순서를 보장하기 위해 사용됩니다.

```jsx
console.log('1');

const setTimeoutWithCallback = (callbackFunc) => {
  setTimeout(() => {
    console.log('2');
    callbackFunc();
  }, 1000);
};

setTimeoutWithCallback(() => console.log('3'));
```

### **🔥 콜백 지옥과 파멸의 피라미드**

코드 순서를 보장하기 위해 콜백 함수를 중첩해서 구현하는 경우 코드 가독성이 떨어지고 이해하기도 어려워지는데 이를 콜백 지옥(callback hell) 또는 파멸의 피라미드(Pyramid of Doom)라고 부릅니다.

이러한 문제를 해결하기 위해 Promise와 async/await와 같은 패턴이 도입되었습니다.

## Promise

콜백 지옥을 해결하기 위해 ES6에서 추가된 문법입니다.

Promise 객체는 비동기 작업의 결과와 상태를 반환하고. 각 상태에 따른 처리를 할 수 있도록 도와줍니다.

Promise 객체가 가진 상태(state) 3가지

- 대기(Pending): 비동기 작업이 아직 완료되지 않은 초기 상태
- 이행(Fulfilled): 비동기 작업이 성공한 상태
- 거부(Rejected): 비동기 작업이 실패한 상태

### 프로미스 객체 생성

```jsx
function asyncFunc() {
  return new Promise((resolve) => {
    // 생성자 함수에 전달되는 함수를 생성자(executor)라고 합니다.
    // 생성자 함수는 인스턴스가 생성될 때 비동기 작업을 수행합니다.
    // 이 코드에서 생성자 함수는 setTimeout 함수를 호출하여 1초 후에 비동기 처리를 수행합니다.

    setTimeout(() => {
      // 비동기 작업이 성공했을 때는 resolve()를 호출하여 결과 값을 함께 전달합니다.
      resolve('async function');
    }, 1000);

    // 비동기 작업이 실패했을 때는 reject()를 호출하여 실패 이유를 함께 전달합니다.
    // reject('error');
  });
}
```

```jsx
asyncFunc()
  .then((value) => {
    // 결과가 fulfilled, 즉 비동기 작업이 성공적으로 수행했을 때 실행될 코드
    // promise에서 반환된 resolve()의 결과값이 인자로 전달됩니다.
    console.log('Data: ', value);
  })
  .catch((error) => {
    // 결과가 rejected, 즉 비동기 작업이 실패했을 때 실행될 코드
    // promise에서 반환된 reject()의 결과값이 인자로 전달됩니다.
    console.error(error);
  });
```

### 프로미스 **체이닝,** then() 그리고 catch()

프로미스 객체는 .then() / .catch()의 메서드 체이닝을 통해 후속 처리를 할 수 있게 됩니다.

then() 메서드에서 반환하는 값은 Promise 객체로, 결과값을 기준으로 이행 / 거부됩니다. 이러한 특성을 이용해 이전 Promise 객체의 값을 이어받아 순차적으로 비동기 작업을 처리할 수 있습니다.

```jsx
asyncFunc()
  .then((resultFromA) => b(resultFromA))
  .then((resultFromB) => c(resultFromB))
  .then((resultFromC) => console.log(resultFromC));
```

### fetch()

실제로 코드를 작성할 때는 `Promise` 객체를 직접 선언하여 사용하는게 아니라, 자바스크립트에 내장된 `Promise` 함수를 사용하게 됩니다.

fetch()는 호출하면 브라우저는 요청을 보내고 Promise를 반환합니다.

```jsx
fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then((response) => response.json())
  .then((data) => console.log(data));
	.catch((error) => console.log(error));
```

하지만 Promise에서도 then() 체인을 길게 이어 나가면 코드의 가독성이 떨어진다는 문제가 있습니다.

## async / await

async / await는 ES2017에서 추가된 문법입니다. 일반적인 동기 코드와 같은 모양으로 작성되기 때문에 위에 소개한 두 가지 방식보다 직관적이고 간결하게 작성할 수 있다는 장점이 있습니다.

### async

- 비동기 로직을 포함하는 함수 앞에 붙이는 예약어
- async가 붙은 함수는 Promise 객체를 리턴합니다.
- await은 asnyc 함수 안에서만 사용이 가능합니다.

### await

- async 키워드를 붙인 함수 안에서만 사용이 가능합니다.
- Promise 객체를 리턴하는 함수 호출 앞쪽에만 붙일 수 있습니다.
- await 키워드를 만나면 코드는 실행을 멈추고, await 뒤에 선언된 Promise 함수가 처리 될 때까지 기다립니다.

```jsx
// 에러 처리는 try…catch 구문을 이용해 처리할 수 있습니다.
const promiseFunc = async () => {
  try {
    // await에서 fulfilled를 반환할 경우 반환된 결과값을 result에 할당
    const result = await asyncFunc();
  } catch (err) {
    // await에서 rejected를 반환할 경우 catch 절로 이동
    console.log(err);
  }
};

promiseFunc();
```
