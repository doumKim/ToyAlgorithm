# hashTable

## 👨🏼‍🦳 문제

<br />

> `insert()`, `retrieve()`, `remove()` 메소드를 가진 해시 테이블을 만드세요.
>
> resize를 구현하지 않아도 테스트는 통과하지만, 충돌은 처리할 수 있어야 합니다.

<br />

## 🤔 풀이

### 🔥 1차 시도 🔥

해시 테이블의 개념부터 짚고 넘어가자면 해시 테이블은 해시 함수를 사용하여 키를 해시 값으로 매핑하고, 이 해시 값을 인덱스로 사용해 value를 저장하는 구조이다.

다만 서로 다른 key가 같은 hash 값을 갖는 경우가 발생하고 이를 해시 충돌이라고 한다.

- 장점

  - key-value가 일대일로 매핑되어 있어 삽입, 삭제, 검색에서 O(1)의 시간복잡도를 가진다.

- 단점

  - 해시 충돌 해결
  - 공간 효율성이 떨어진다. => 데이터가 저장되기 전에 미리 저장 공간을 확보해야한다.
  - 해시 함수의 의존도가 높다.

- 충돌 해결방법
  - Separate Chaining (연결리스트 사용)
  - Open Addressing (비어 있는 버킷을 찾아서 데이터 저장)

위의 정리한 개념들을 기반으로 insert 메서드의 코드 전략을 짜보자면

1. 제공되는 getIndexBelowMaxForKey 함수를 이용하여 해시 값을 구한다.
2. storage의 hash 인덱스에 값이 존재하는지 확인한다.
3. 있다면? key, value 버킷을 추가한다.
4. 없다면? 버킷 객체를 만들어 key, value 넣어주고 storage에 hash 인덱스에 넣어준다.

다음으로 retrieve 메서드의 코드 전략을 짜보면

1. 마찬가지로 해시 값을 구한다.
2. storage의 hash 인덱스에 값이 있는지 확인하고
3. 있다면? storage의 hash 인덱스의 key 프로퍼티를 반환
4. 없다면? undefined를 반환한다.

마지막으로 remove 메서드를 전략을 구현해보자

1. 마찬가지로 해시 값을 구한다.
2. storage의 hash 인덱스의 key 프로퍼티를 삭제한다.

```js
const makeHashTable = function () {
  let result = {};
  let storage = [];
  let storageLimit = 1000;
  result.insert = function (key, value) {
    const hash = getIndexBelowMaxForKey(key, storageLimit);
    if (storage[hash]) {
      storage[hash][key] = value;
    } else {
      const bucket = {};
      bucket[key] = value;
      storage[hash] = bucket;
    }
  };
  result.retrieve = function (key) {
    const hash = getIndexBelowMaxForKey(key, storageLimit);
    if (storage[hash]) {
      return storage[hash][key];
    } else {
      return undefined;
    }
  };
  result.remove = function (key) {
    const hash = getIndexBelowMaxForKey(key, storageLimit);
    delete storage[hash][key];
  };

  return result;
};

// 이 함수는 "해시 함수" 입니다. 지금은 이 함수에 대해 걱정할 필요가 없습니다.
// 해시 함수는 문자열과 숫자 max를 인자로 받아, 문자열을 0에서 max-1 사이에 분포된 정수로 바꿔 리턴합니다.
const getIndexBelowMaxForKey = function (str, max) {
  let hash = 0;
  for (let i = 0; i < str.length; i++) {
    hash = (hash << 5) + hash + str.charCodeAt(i);
    hash = hash & hash; // Convert to 32bit integer
    hash = Math.abs(hash);
  }
  return hash % max;
};
```

성공..! 🍾 🎊 🎉

---

## 정리

자료구조에 대한 개념을 잊기 쉬운데 다시 한번 머릿속에서 상기시킬 수 있어 좋았다.
