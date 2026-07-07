# 09강. 객체

## 학습 목표

- 관련 있는 값을 하나로 묶는 객체를 이해합니다.
- 속성과 메서드를 구분합니다.
- 점 표기법으로 값을 읽고 수정합니다.

## 쉬운 설명

객체는 하나의 대상에 대한 정보를 묶어 놓은 것입니다.

```js
const user = {
  name: "민수",
  age: 20,
  isStudent: true
};
```

`name`, `age`, `isStudent`는 속성입니다.

## 값 읽기

```js
console.log(user.name);
console.log(user.age);
```

## 값 수정하기

```js
user.age = 21;
console.log(user.age);
```

## 메서드

객체 안에 들어 있는 함수는 메서드라고 부릅니다.

```js
const dog = {
  name: "초코",
  bark: function () {
    console.log("멍멍!");
  }
};

dog.bark();
```

## 예제 코드

예제 파일:

```txt
09-객체/examples/objects.html
```

## 실행 방법

1. `objects.html`을 엽니다.
2. 사용자 정보가 화면에 표시되는지 확인합니다.
3. 객체의 값을 바꿔 봅니다.

## 자주 하는 실수

- 쉼표를 빼먹음
- 문자열 값에 따옴표를 빼먹음
- 객체와 배열을 같은 것으로 생각함

## 연습문제

1. 자신의 이름, 나이, 취미를 가진 객체를 만드세요.
2. 이름을 출력하세요.
3. 나이를 1 증가시키세요.
4. 자기소개를 출력하는 메서드를 추가하세요.

## 정답 예시

```js
const me = {
  name: "지수",
  age: 20,
  hobby: "독서",
  introduce: function () {
    console.log("안녕하세요. 저는 " + this.name + "입니다.");
  }
};

me.introduce();
```

## 요약

- 객체는 관련 있는 정보를 묶어 둡니다.
- 속성은 객체의 값입니다.
- 메서드는 객체 안의 함수입니다.

## 다음 강의

[10강. 배열과 객체 메서드](../10-배열과객체-메서드)
