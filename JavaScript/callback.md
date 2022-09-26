# 콜백 함수 (Callback Function)

## 콜백 함수란?

> 파라미터로 함수를 전달받아, `함수의 내부`에서 실행하는 함수입니다.

## 콜백 함수 예시

```js
function myName(lastname, firstname, callback) {
  const fullname = lastname + firstname;

  //함수 내부에서 실행
  callback(fullname);
}

myName("홍", "길동", function (name) {
  console.log(name); //홍길동
});
```

## 콜백 함수 동작 방식 비유

맛집에 갔다가 대기 줄이 길어서 대기자 명단을 썼습니다. 입장하기 전까지 주변을 구경 다닙니다. 그러던 중 식당에 자리가 났다고 전화가 왔습니다. <br />
이때, 전화를 받는 시점이 콜백 함수가 호출되는 시점입니다. 자리가 준비된 시점에 원하는 동작을 수행할 수 있습니다.

## 콜백 지옥

비동기 처리 로직을 위해 콜백 함수를 연속해서 사용하면 콜백 지옥이 시작됩니다.

```js
function callback() {
  setTimeout(() => {
    console.log("1");
    setTimeout(() => {
      console.log("2");
      setTimeout(() => {
        console.log("3");
      }, 0);
    }, 0);
  }, 0);
}

callback(); //🔥
```

## 콜백 지옥의 문제점

1. 가독성이 떨어집니다.
2. 에러가 해결하거나 디버깅 하기 어렵습니다.
