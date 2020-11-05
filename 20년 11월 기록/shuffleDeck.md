# shuffleDeck

## 👨🏼‍🦳 문제

트럼프 카드로 이루어진 배열이 주어졌을 때, 그 카드들을 섞는 함수를 작성하세요.

> **예시:**
>
> ```js
> const deck = orderedDeck();
> // ["A♥","2♥","3♥",...,"J♦","Q♦","K♦"]
> shuffleDeck(deck);
> // ["2♠","J♣","A♦", ... ,"7♣","8♣","K♠"]
> ```

정렬 상태 카드덱 생성기

```js
const orderedDeck = function () {
  let suits = ['♥', '♣', '♠', '♦'];
  let values = ['A', 2, 3, 4, 5, 6, 7, 8, 9, 10, 'J', 'Q', 'K'];
  let deck = [];

  suits.forEach(function (suit) {
    values.forEach(function (value) {
      deck.push(value + suit);
    });
  });

  return deck;
};
```

참고:
카드 덱은 완전히 무작위로 섞여져야 합니다. 즉 주어진 모든 카드는 각자 입력되었을 때 가지고 있던 인덱스와 상관없는
완전히 독립적인 인덱스 번호에 해당하는 자리에 나타날 가능성이 있어야 합니다.
여러분의 알고리즘은 덱을 제대로 섞고 있나요? 알고리즘 편향이 일어나고 있진 않나요?

Advanced:

아주 간단한 알고리즘조차도 선형적인(linear time) 시간 복잡도를 가집니다.
여러분의 알고리즘은 배열이 N만큼 늘어나도 여전히 비편향을 유지할 수 있나요?
선형적인 시간 복잡도를 가지는 비편향적인 무작위 알고리즘을 만드는 데 성공했다면,
공간 복잡도(https://en.wikipedia.org/wiki/Space_complexity)를 한번 생각해 보시겠어요?
O(N) 시간 복잡도와 O(1) 공간 복잡도를 가지는 알고리즘을 한번 설계해 보세요.
무작위성에 대한 참고 설명:
컴퓨터가 만드는 난수는 주로 의사난수일 가능성이 높습니다.
(의사난수 : https://ko.wikipedia.org/wiki/%EC%9C%A0%EC%82%AC%EB%82%9C%EC%88%98 ,
https://ko.wikipedia.org/wiki/%EB%82%9C%EC%88%98)
정말 무작위로 정해진 난수와 의사난수의 차이점은 아주 작아서 테스트로 판별하기가 어렵습니다.
그러나 차이점은 있습니다. 여러분은 주로 Math.random()을 이용해서 난수를 생성하셨을 것입니다.
이 함수는 현재 시간 millisecond를 이용해서 난수를 생성하기 때문에 컴퓨터 시간을 난수를 생성했던 시간으로 되돌릴 경우 이전과 똑같은 난수를 생성하게 됩니다.

<br />

## 🤔 풀이

### 🔥 1차 시도 🔥

카드를 어떻게 섞어야 중복되지 않을까 고민을 먼저 해보았고 deck의 개수만큼 반복을 시키고 난수를 발생 시켜 0부터 끝까지 랜덤한 자리에 넣어주는 방식을 택하였다.

하지만 Advanced로 주어진 조건을 만족하지는 못했다. 문제점을 파악을 해보니 아래의 코드에서 난수를 발생할 때 이미 섞은 카드의 자리까지 다시 발생하는 문제점이 있었다.

따라서 1차 시도 실패

```js
const shuffleDeck = function (deck) {
  for (let i = 0; i < deck.length; i++) {
    const randomInt = Math.floor(Math.random() * deck.length);
    const temp = deck[i];
    const randomPick = deck[randomInt];

    deck[i] = randomPick;
    deck[randomInt] = temp;
  }
  return deck;
};
```

### 🔥 2차 시도 🔥

그렇다면 난수를 어떻게 만들어야 이미 섞은 카드를 다시 뽑지 않을까 고민하니 이러한 규칙을 만들었다.

만약 10개의 카드가 있다고 가정을 하면 n번의 shuffle 마다

1. 0 + ( 0 ~ 9 - 0) => 0 ~ 9
2. 1 + ( 0 ~ 9 - 1) => 1 ~ 9
3. 2 + ( 0 ~ 9 - 2) => 2 ~ 9

즉 이미 섞인 카드를 다시 섞는 행위를 하지 않아도 된다. 아래의 코드로 테스트를 돌리니 성공.

```js
const shuffleDeck = function (deck) {
  for (let i = 0; i < deck.length; i++) {
    const randomInt = i + Math.floor(Math.random() * (deck.length - i));
    const temp = deck[i];
    const randomPick = deck[randomInt];

    deck[i] = randomPick;
    deck[randomInt] = temp;
  }
  return deck;
};
```

---

## 정리

항상 시간복잡도를 생각하면서 로직을 짜야겠다. 또한 정렬은 알고리즘 문제의 거의 기초이자 필수이니 꼭 완벽하게 마스터하자.
