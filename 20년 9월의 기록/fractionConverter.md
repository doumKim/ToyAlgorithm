# fractionConverter

## 👨🏼‍🦳 문제

> 숫자를 매개변수로 받아서 문자열 형태의 분수로 리턴하는 함수를 작성하세요.
>
> ```js
> Example: toFraction(0.5) === '1/2';
> ```
>
> 정수와 대분수들은 가분수로 바꿔서 리턴해야 합니다. (가분수 : 분자가 분모보다 크거나 같은 분수)
>
> ```js
> Example: toFraction(3.0) === '3/1';
>
> Example: toFraction(2.5) === '5/2';
> ```

## 🤔 풀이

### 🔥 1차 시도 🔥

1차 시도라서 무식했다는 사실 변명이지만 굉장히 무식한 방법을 사용했다.

Example를 살펴보면 0.5 => 0.5에 어떤 값 x를 곱 했을 때 정수가 나온다면 0.5 \* x / x === 0.5 라는 단순 무식한 규칙이 있다.

_(사실 너무 당연한거라 규칙이라고 하기도 조금 부끄럽네..)_

그럼 while문을 사용해서 가장 정수를 도출하는 가장 작은 x가 나올 때 까지 반복을 돌리고

값들을 이용해서 결과물을 반환을 했다.

근데 여기서 성능적인 문제점 보다 더 큰 문제점이 발견되었다.

자바스크립트에서 0.1 + 0.1 + 0.1 !== 0.3 인걸 기억하고 있는가... 그렇다 부동소수점...

애초에 while 문을 사용하는 단순 무식 방법도 마음에 들지 않지만 문제 해결도 불가능했다.

---

### 🔥 2차 시도 (해결) 🔥

이번에는 생각을 바꿔서 0.5를 즉, 5 / 10 -> 1 / 2 으로 표현하는 방법이 뭐가 있을까에 집중을 했다.

떠오른 방법은 0.5를 곱하기 10을 하고 정수 5로 만들어서 그 5와 10의 최대 공약수 5로 나눠주면 1 / 2 로

표현이 가능하다는걸 캐치했고 나는 이걸 적용하기 위해 최대 공약수를 구하는 로직부터 작성을 하였다.

```js
const getGCDNumber = (a, b) => {
  if (a % b === 0) return b;
  return getGCDNumber(b, a % b);
};
```

숫자 2개를 입력하면 % 연산자를 통해 나머지가 0 인지 확인하고 그게 참이라면 바로 b를 반환한다.

그게 아니라면 재귀호출을 통해 만들어 놓은 로직을 한번 더 동작시키고 또 조건문을 타게한다.

그렇게 최대 공약수를 구하는 로직을 넣어 정답을 도출하는 전체적인 로직을 작성하면 다음과 같다.

```js
const toFraction = function (number) {
  let isPositive = number >= 0;

  number = Math.abs(number);
  if (Number.isInteger(number)) return `${number}/1`;

  let decimalPlaces = number.toString().match(/.\d+/)[0].length - 1;
  number = Math.floor(number * Math.pow(10, decimalPlaces));

  const getGCDNumber = (a, b) => {
    if (a % b === 0) return b;
    return getGCDNumber(b, a % b);
  };

  let gcdNumber = getGCDNumber(number, Math.pow(10, decimalPlaces));
  if (isPositive)
    return `${number / gcdNumber}/${Math.pow(10, decimalPlaces) / gcdNumber}`;
  return `-${number / gcdNumber}/${Math.pow(10, decimalPlaces) / gcdNumber}`;
};
```

처음에는 isPositive라는 변수를 만들지 않았는데 음수가 입력으로 들어왔을 때 제대로 동작을 하지 않아서 분기처리를 하기 위해 변수를 만들어 줬고 결과값은 부호만 다르기 때문에 절대값 처리를 해주고 시작을 했다.

사실 여기서 제일 고민이 많았던 부분은 부동소수점을 인해서 제대로 10^n을 곱해서 정수를 만들지 못한다는 점이다.

따라서 나는 약간의 편법처럼 보이지만 정규표현식을 사용해서 소수점의 자릿수를 구한 후 10^n을 곱해줬다.

그 후에 Math.floor로 나머지 소수점을 지워줬다. 이 방법으로 예상에서 벗어난 결과값을 방지할 수 있었다.

---

## 정리

일단 코드가 지저분해보인다.... 사실 별로 마음에 들지는 않는다.

뭔가 더 좋은 방법이 있을거 같은데 아직까지는 떠오르지 않는다. 꼭 다시 살펴봐야지.

특히 소수점을 구하기 위해서 정규표현식이 아닌 다른 방법이 있는지 고민해봐야겠다.
