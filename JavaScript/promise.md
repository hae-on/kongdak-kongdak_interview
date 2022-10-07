# Promise

## 비동기란?

비동기 처리란 '특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는 것'을 말합니다.

예를 들어 당신이 서빙 중이라고 생각해봅시다.

동기적 처리라면 1번 손님의 주문을 받고, 음식이 나올 때까지 기다렸다가 서빙해야 합니다.

그러나 이는 비효율적이죠.

음식이 나올 때까지 빈 시간에 다른 손님의 주문을 받는 것이 효율적입니다.

이를 비동기적 처리 방식이라고 합니다.

## Promise 필요성

프로미스는 자바스크립트의 비동기 처리에 사용합니다.

주로 서버에서 받아온 데이터를 화면에서 표시할 때 이용합니다.

서버에 데이터를 보내달라는 요청을 보낸다고 가정해봅시다.

이때, 데이터를 받기도 전에 화면에 데이터를 표시하려고 하면 오류가 발생하죠.

이와 같은 문제를 해결하기 위해 프로미스를 사용합니다.

### Promise의 상태

프로미스에서는 3가지 상태가 있습니다.

- Pending(대기): 비동기 처리 로직이 미완료 상태
- Fulfilled(이행): 비동기 처리가 완료되어 프로미스가 결괏값 반환해준 상태
- Rejected(실패): 비동기 처리가 실패하거나 오류가 발생한 상태

1. Pending(대기)

```js
const promise = new Promise();
```

new라는 키워드를 통해서 object를 생성해줍니다.

promise를 호출하면 대기 상태가 됩니다.

```js
const promise = new Promise(function(resolve, reject)){
  // 내용
}
```

promise 메서드를 호출할 때 콜백 함수를 선언할 수 있습니다.

이때 resolve와 reject를 인자로 받습니다.

resolve는 기능을 정상적으로 수행해서 최종 데이터를 전달할 때 사용합니다.

반면 reject는 기능을 수행하다 중간에 문제가 생길 때 사용합니다.

2. Fulfilled(이행) = 완료

```js
const promise = new Promise(function(resolve, reject)){
  resolve()
}
```

콜백 함수의 인자인 resolve를 실행하면 이행 상태에 들어옵니다.

```js
function getData() {
  return new Promise(function (resolve, reject) {
    let name = "홍길동";
    resolve(name);
  });
}

getData().then(function (resolvedData) {
  console.log(resolvedData); // 홍길동
});
```

이행 상태에 then을 이용하면 처리 후 결괏값을 받을 수 있습니다.

3. Rejected(실패)

```js
const promise = new Promise(function(resolve, reject)){
  reject()
}
```

콜백 함수의 인자인 resolve를 실행하면 실패 상태가 됩니다.

```js
function getData() {
  return new Promise(function (resolve, reject) {
    reject(new Error("실패"));
  });
}

getData()
  .then()
  .catch(function (error) {
    console.log(error);
  });
```

실패 상태에 catch를 이용하면 실패한 원인을 받을 수 있습니다.

## Promise Chaining

프로미스는 여러 개를 연결하여 사용할 수 있습니다.

```js
new Promise(function (resolve, reject) {
  setTimeout(() => resolve(1), 1000);
  // (*) 1초 후 최초 프로미스가 이행됩니다.
})
  .then(function (result) {
    // (**) 첫 번째 then이 호출됩니다.

    alert(result); // 1
    return result * 2;
  })
  .then(function (result) {
    // (***) (**)에서 반환한 값은 다음 then에 전달됩니다.

    alert(result); // 2
    return result * 2;
  })
  .then(function (result) {
    alert(result); // 4
    return result * 2;
  });
```

resolve()가 호출되면 프로미스는 대기 상태에서 이행 상태로 넘어갑니다.

따라서 첫 번째 then으로 넘어갑니다.

첫 번째 then에서는 이행된 결괏값을 받아 계산하고 다음 then으로 넘겨줍니다.

계속해서 이전 프로미스의 값을 받아 계산하고 넘겨주는 방식으로 진행됩니다.

## Promise의 에러 처리 방법

1. then의 두 번째 인자로 에러 처리

```js
getDate().then(
  handleSuccess, //
  handleError
);
```

2. catch()를 이용

```js
getData().then().catch();
```

두 가지 모두 프로미스의 로직이 정상적으로 돌아가지 않을 때 호출됩니다.

```js
function getData() {
  return new Promise(function (resolve, reject) {
    reject(new Error("실패"));
  });
}

// 1. then() 두 번째 인자로 전달
getData().then(
  function () {
    //
  },
  function (error) {
    console.log(error);
  }
);

// 2. catch 사용
getData()
  .then()
  .catch(function (error) {
    console.log(error);
  });
```

### 둘 중에 어느 것으로 에러 처리?

가급적 catch()를 사용하는 것이 좋습니다.

catch()는 프로미스에 발생한 모든 에러를 다루기 때문입니다.
