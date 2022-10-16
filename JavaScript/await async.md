# await & async

> 비동기 처리에 사용합니다.

## await & async 사용 이유

> 기존의 처리 방식의 문제점을 보완하고, 사용법이 훨씬 간단합니다.

- Promise 코드

```js
function greeting() {
  return new Promise((resolve, reject) => {
    resolve('hello');
  });
}

greeting().then((i) => console.log(i));
```

- async 코드

```js
async function greeting() {
  return 'hello';
}

greeting().then((i) => console.log(i));
```

함수에 async를 붙이면 자동으로 promise 객체로 인식됩니다.

return 값은 resolve() 값과 동일합니다.

## await & async 기본 문법

```js
async function 함수명() {
  await 비동기처리메서드명();
}
```

함수의 앞에 async라는 예약어를 붙입니다.

비동기로 처리되는 부분 앞에 await를 붙입니다.

async가 붙은 함수는 promise를 반환하고, promise가 아닌 것은 promise로 감싸 반환합니다.

## async의 예외 처리 방법

- then과 catch 사용

```js
async function funcError() {
  throw 'error';
}

funcError()
  .then((i) => console.log(i))
  .catch((i) => console.log(i));
```

- try와 catch 사용

```js
function funcError() {
  return new Promise((resolve, reject) => {
    reject('Error!!');
  }, 1000);
}

(async function funcError2() {
  try {
    await funcError();
  } catch (e) {
    console.error(e);
  }
})();
```

## await의 최상위 레벨 코드

최상위 레벨 코드에서 await를 사용할 수 없습니다.

- 불가능 예시

```js
let response = await fetch('/..');
let user = await response.json();
```

하지만 익명 async 함수로 코드를 감싸면 최상위 레벨 코드에서도 await를 사용할 수 있습니다.

- 가능 예시

```js
(async () => {
  let response = await fetch('/..');
  let user = await response.json();
})();
```
