# evenOccurrence

## 👨🏼‍🦳 문제

<br />

> 배열이 주어졌을 때, 값이 같은 원소가 짝수 개수만큼 나오는 첫 번째 원소를 리턴하세요.
> 개수가 짝수인 원소들이 여러 개면, 그 중 첫 번째 원소를 리턴하세요.
> 개수가 짝수인 원소가 없으면, null을 리턴하세요.
>
> **예시:**
>
> ```js
> let onlyEven = evenOccurrence([1, 7, 2, 4, 5, 6, 8, 9, 6, 4]);
> console.log(onlyEven); //  4
> ```

<br />

## 🤔 풀이

### 🔥 1차 시도 🔥

일단 문제를 보자마자 든 생각은 반복문 안에서 reduce 함수를 돌려 중복이 된다면 카운트를 하나씩 증가 시켜서 풀면 간단하게 해결될거 같다는 생각이 들었다.

바로 적용을 해보자.

```js
const evenOccurrence = function (arr) {
  for (let i = 0; i < arr.length; i++) {
    const inspectedNumber = arr[i];
    const occurrenceTimes = arr.reduce((count, currentNumber) => {
      return inspectedNumber === currentNumber ? count + 1 : count;
    }, 0);

    if (occurrenceTimes % 2 === 0) {
      return inspectedNumber;
    }
  }
};
```

성공..! 🍾 🎊 🎉

but.. 시간 복잡도가 O(n^2) 이라는게 뭔가 마음에 들지 않았다.

### 🔥 2차 시도 🔥

이번에는 해시 테이블을 이용하면 2중 반복을 없앨 수 있을거라고 생각이 들었다.

따라서 이번에는 해시 테이블을 하나 만들고 프로퍼티로 요소 하나씩을 넣어주는데 값이 이미 있다면

논리값을 반전 시켜주는 방식을 취했다.

그리고 한번 더 반복문을 돌려서 순차적으로 해시 테이블 안에서 true 값을 가진 프로퍼티를 검사하는 방식으로 결과 값을 도출했다.

```js
const evenOccurrence = function (arr) {
  const hashTable = {};

  arr.forEach((number) => {
    hashTable[number] = !hashTable[number];
  });

  for (let i = 0; i < arr.length; i++) {
    if (!hashTable[arr[i]]) return arr[i];
  }

  return null;
};
```

따라서 시간 복잡도를 O(n^2) => O(n) 으로 줄일 수 있게 되었다.

---

## 정리

항상 내가 짠 코드보다 더 좋은 방법이 있을 거 라고 염두하고 고민을 하니 더 좋은 방법이 생각이 났고 정렬이나 자료구조 등의 단골 핵심 개념의 중요함을 또 깨달았다.
