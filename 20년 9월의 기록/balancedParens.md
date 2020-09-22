# balancedParens

## 👨🏼‍🦳 문제

> 1단계 : 문자열이 input으로 주어졌을 때, 문자열 안의 모든 괄호의 짝이 맞는지 아닌지를 return하는 함수를 구현하세요.

```js
balancedParens('('); // false
balancedParens('()'); // true
balancedParens(')('); // false
balancedParens('(())'); // true
```

> 2단계 : 모든 종류의 괄호에도 작동하도록 구현하세요.

```js
balancedParens('[](){}'); // true
balancedParens('[({})]'); // true
balancedParens('[(]{)}'); // false
```

> 3단계: 괄호가 아닌 문자는 무시하세요.

```js
balancedParens(' let wow  = { yo: thisIsAwesome() }'); // true
balancedParens(' let hubble = function() { telescopes.awesome();'); // false
```

## 🤔 풀이

### 🔥 1차 시도 (미해결) 🔥

일단 나는 문제를 보자마자 정규표현식을 사용하면 뭔가 쉽게 해결할 수 있지 않을까? 라는 생각을 했다.

그래서 input을 배열로 바꿔주고 reduce 메소드를 사용해서 괄호가 열리거나 닫히면 객체에 프로퍼티를 추가하고 +1, -1을 해주려고 계획을 했다.

그러고 나서 순회를 하며 그 프로퍼티의 값으로 괄호가 잘 열리고 닫히는지 체크를 하려고 했다.

근데 작성을 해보고 나니 테스트를 모두 통과하지 못했다.

실패 요인은 일단 2단계에서 불규칙하게 나열된 괄호를 false 처리하지 못했다.

```js
const balancedParens = function (input) {
  const splitedInput = input.split('');
  let result = true;
  const temp = splitedInput.reduce((acc, cur) => {
    let openBracket = cur.match(/[{[(]/g);
    let closeBracket = cur.match(/[\]})]/g);
    if (openBracket) {
      if (!acc[openBracket[0]]) {
        acc[openBracket[0]] = 0;
      } else {
        acc[openBracket[0]] -= 1;
      }
    }
    if (closeBracket && closeBracket[0] === '}') {
      if (acc['{'] !== undefined) {
        acc['{'] += 2;
      }
    } else if (closeBracket && closeBracket[0] === ']') {
      if (acc['['] !== undefined) {
        acc['['] += 2;
      }
    } else if (closeBracket && closeBracket[0] === ')') {
      if (acc['('] !== undefined) {
        acc['('] += 2;
      }
    }
    return acc;
  }, {});

  if (Object.keys(temp).length === 0) {
    return false;
  }
  for (let key in temp) {
    if (temp[key] <= 0 || temp[key] % 2 !== 0) {
      result = false;
    }
  }
  return result;
};
```

---

### 🔥 2차 시도 (해결) 🔥

2차 시도에서는 스택을 이용해보면 어떨까? 라는 생각을 했고 입력받은 문자열 반복문을 돌려서

괄호가 열리면 스택에 push, 괄호가 닫히면 스택에 pop을 해주고 pop한 문자를 recent 변수에 저장하고

parens 객체에서 recent 밸류를 조회하고 그 값이 현재 문자와 같은지 확인을 해준다.

그리고 이 반복 작업을 해주고 나서 스택이 모두 비워졌으면 짝이 올바르게 맞기 때문에 true를 반환

스택에 요소가 남아 있다면 짝이 맞지 않았기 때문에 false를 반환해줬다.

```js
const balancedParens = function (input) {
  // stack을 이용
  const stack = [];
  const parens = {
    '(': ')',
    '{': '}',
    '[': ']',
  };

  for (let i = 0; i < input.length; i++) {
    // 괄호가 열리면 스택에 푸쉬
    if (input[i] === '(' || input[i] === '{' || input[i] === '[') {
      stack.push(input[i]);
    }
    // 괄호가 닫히면 스택에서 팝
    else if (input[i] === ')' || input[i] === '}' || input[i] === ']') {
      // 팝된 괄호를 저장
      let recent = stack.pop();
      // 괄호의 짝이 맞는지 확인하고 틀리면 false
      if (input[i] !== parens[recent]) {
        return false;
      }
    }
  }
  // 스택이 다 비워졌으면 true 반환 아니면 false 반환
  if (stack.length !== 0) {
    return false;
  } else {
    return true;
  }
};
```

결과는 테스트 통과..!

---

## 정리

문자열만 보면 어떻게 정규표현식으로 해결하려는 버릇을 고쳐야겠다.

정규표현식이 물론 편하지만 문제에서 요하는 개념을 접목시켜서 해결하는게 지금은 더 좋은 방법같다.
