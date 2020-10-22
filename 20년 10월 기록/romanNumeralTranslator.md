# romanNumeralTranslator

## 👨🏼‍🦳 문제

> 로마 숫자가 주어졌을 때, 이 로마 숫자를 아라비아 숫자로 바꿔 출력하는 함수를 작성하세요.
>
> **예시:**
>
> ```js
> translateRomanNumeral("LX"); // 60
> ```
>
> 로마 숫자에서는 큰 수 앞에 작은 수를 놓았을 때큰 수에서 작은 수를 뺀 값을 표시한 것으로 판단합니다. (감산 표기법)큰 수 앞에는 작은 수가 오직 하나만 올 수 있다고 가정합니다.
>
> **예시:**
>
> ```js
> translateRomanNumeral("IV"); // 4
> ```
>
> 로마 숫자 이외의 것들이 인자로 주어졌을 때는 null을 리턴해야 합니다.

<br />

## 🤔 풀이

### 🔥 1차 시도 🔥

일단 문제를 보니 translateRomanNumeral 함수의 인자 값을 split 해서 배열로 만들어서 처리를 해야겠다는 생각이 들었다.

또한 그 배열에 map 함수를 돌려서 아래의 DIGIT_VALUES 객체 안에 있는 value 값으로 변환을 시키는데 위에 주워진 조건을 걸어서 양수로 반환을 할지 음수로 반환을 할지 결정을 했다.

조건에 따르면 두 수를 비교해야 하기 때문에 배열의 index+1 값이 존재하면 위에서 말한 로직을 통하게 하고 그게 아니라면 무조건 마지막 값은 양수이기 때문에 그대로 반환을 시켜줬다.

그리고 마지막으로 map 함수를 돌려서 나온 배열을 다시 reduce 메소드를 통해 총 합을 구해주고 반환을 시켜줬다.

따라서 성공..! 🍾 🎊 🎉

```js
const DIGIT_VALUES = {
  I: 1,
  V: 5,
  X: 10,
  L: 50,
  C: 100,
  D: 500,
  M: 1000,
};

const translateRomanNumeral = function (romanNumeral) {
  if (typeof romanNumeral !== "string") {
    return null;
  }
  const splited = romanNumeral.split("");

  const mapped = splited.map((value, index) => {
    if (splited[index + 1]) {
      if (DIGIT_VALUES[value] >= DIGIT_VALUES[splited[index + 1]]) {
        return DIGIT_VALUES[value];
      } else {
        return -DIGIT_VALUES[value];
      }
    }
    return DIGIT_VALUES[value];
  });

  return mapped.reduce((acc, cur) => {
    return (acc += cur);
  }, 0);
};
```

---

## 정리

input 값으로 다른 타입의 값이 아닌 문자열이지만 DIGIT_VALUES에 없는 값을 넣었을 때의 처리도 시도를 해보면 좋을 것 같다.

나름 쉽게 풀려서 리팩토링에 시간을 조금 더 써야겠다.
