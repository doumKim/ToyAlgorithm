# rotateMatrix

## 👨🏼‍🦳 문제

> NxN 매트릭스를 90도로 회전시키는 함수를 작성하세요.

매트릭스는 2차원 배열(배열의 배열)로 표현됩니다.

```js
1x1 매트릭스 예시:
  [ [1] ]

2x2 매트릭스 예시:
[ [1,2],
  [3,4] ]
```

참고: 수학, 그리고 컴퓨터 과학에서 행렬은 "행"을 뜻하는 m과 "열"을 뜻하는 n의 곱으로 설명됩니다. 따라서 행렬의 i 주소는 "세로축에서 i만큼 아래에 있고 가로축에서 j만큼 옆에 있다." 를 뜻합니다. 이 방식은 일반적으로 배열이 코드에서 처리되는 방식과 일치하지만, 기하학이나 컴퓨터 그래픽에서는 (x, y) 형식의 좌표가 "가로축에서 x만큼 옆에 있고 세로축에서 y만큼 아래에 있다"를 뜻합니다.

```js
4x4 매트릭스 회전의 예시:

const matrix = [
[1,2,3,4],
[5,6,7,8],
[9,'A','B','C'],
['D','E','F','G']
];
matrix[0][0]; // 1
matrix[3][2]; // 'F'

const rotatedMatrix = rotateMatrix(matrix); // Rotate 90 degrees clockwise
// rotatedMatrix is:
[ ['D',9,5,1],
['E','A',6,2],
['F','B',7,3],
['G','C',8,4]
]

rotatedMatrix[0][0]; // 'D'
rotatedMatrix[3][2]; // 8
```

추가 과제:

이 함수가 직사각형 매트릭스에서도 작동하도록 구현하세요. (NxN이 아닌 MxN)
이 함수가 두 번째 전달인자로 회전의 방향을 달리할 수 있도록 구현하세요. (1 = 시계 방향, -1 = 반시계 방향)

## 🤔 풀이

### 🔥 1차 시도 (미해결) 🔥

일단 1차 시도이기 때문에 직사각형 배열이나 반 시계 방향 회전은 생각하지 않고 시도를 했다.

규칙을 찾기 위해서 경우의 수를 따져봤다.

```js
matrix[0][0] -> matrix[0][3]
matrix[0][1] -> matrix[1][3]
matrix[0][2] -> matrix[2][3]
matrix[0][3] -> matrix[3][3]
matrix[rowIndex][colIndex] -> matrix[colIndex][length - rowIndex]

matrix[1][0] -> matrix[0][2]
matrix[1][1] -> matrix[1][2]
matrix[1][2] -> matrix[2][2]
matrix[1][3] -> matrix[3][2]
matrix[rowIndex][colIndex] -> matrix[colIndex][length - rowIndex]
```

2차원 배열이니 2중 반복을 해서 행과 열을 하나씩 순회할 수 있다.

```js
matrix[rowIndex][colIndex] -> matrix[colIndex][length - rowIndex]
```

이런 규칙이 존재한다는걸 확인했고 코드를 작성했다.

일단 결과물이 담길 matrix를 만들어주고 시작을 했는데 그것도 우여곡절이 있었다.

멍청하게 참조를 생각하지 않고

```js
const matrix = new Array(4).fill([]);
```

이런식으로 2차원 배열을 만들어..... 후....

```js
const rotateMatrix = function (matrix, direction) {
  if (matrix.length === 0) return matrix;
  const matrixLength = matrix.length - 1;
  let result = new Array(matrixLength + 1);
  for (let i = 0; i <= matrixLength; i++) {
    result[i] = [];
  }
  if (direction === 1) {
    matrix.forEach((row, colIndex) => {
      row.forEach((col, rowIndex) => {
        result[rowIndex][matrixLength - colIndex] = col;
      });
    });
  }
  return result;
};
```

근데 문제에서 원하는 결과 값이 나오기는 하는데 코플릿 테스트 결과는 fail이 뜬다....

direction이 1 그리고 n\*n matrix의 테스트 케이스인데 통과를 못하는 거 보면 내 코드가 테스트에서는

이상하게 동작한다는 뜻인데...

무튼 아직은 fail..

---

## 정리

아직은 해결 방안을 찾지 못했고 아마 내일은 SQL 스프린트 때문에 정신이 없을테니 주말을 통해서

문제점을 해결해 봐야겠다.
