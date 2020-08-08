# Jest
### 概要
- シンプルなテスティングフレームワーク、と公式が主張している
- _______が得意

### 始め方
- 以下のコマンドを打ってJestを有効にする

```shell
$ yarn add jest --dev
```

### サンプル
- テスト対象となるコード
```javascript
function sum(a, b) {
  return a + b;
}

function sub(a, b) {
  return a - b;
}

function mul(a, b) {
  return a * b;
}

function div(a, b) {
  return a / b;
}

function inv(a, n) {
  return Math.pow(a, n);
}

module.exports = {
  sum,
  sub,
  mul,
  div,
  inv,
};
```

- テストコード。拡張子は `.test.js`とすること。

```javascript
const { sum, sub, mul, div, inv } = require('./sum');

test('1 + 2を計算し、3を出力する', () => {
  expect(sum(1, 2)).toBe(3);
});

test('200000000 + 1000000000を計算し、1200000000を出力する。', () => {
  expect(sum(200000000, 1000000000)).toBe(1200000000);
});

test('3 - 4を計算し、-1を出力する', () => {
  expect(sub(3, 4)).toBe(-1);
});

test('653180531098475310 - 780320895328957を計算し、652400210203146400を出力する', () => {
  expect(sub(653180531098475310, 780320895328957)).toBe(652400210203146400);
});

test('300 * 1.23を計算し、369を出力する', () => {
  expect(mul(300, 1.23)).toBe(369);
});

test('100 * 0.53を計算し、53を出力する', () => {
  expect(mul(100, 0.53)).toBe(53);
});

// 浮動小数点を使うときはtoBeCloseToを用いる
test('4 / 3を計算し、1.33333を出力する', () => {
  expect(div(4, 3)).toBeCloseTo(1.33333333333333333333333);
})

test('5 / 2を計算し、2.5を出力する', () => {
  expect(div(5, 2)).toBeCloseTo(2.5);
})

test('2 ^ 3を計算し、8を出力する', () => {
  expect(inv(2, 3)).toBe(8);
})

test('99 ^ 3289を計算し、90438207500880450000を出力する', () => {
  expect(inv(99, 10)).toBe(90438207500880450000);
})
```

- テスト実行は `$ yarn test` をする。

```shell
yarn run v1.22.0$ jest
 PASS  ./sum.test.js
  ✓ 1 + 2を計算し、3を出力する (3 ms)
  ✓ 200000000 + 1000000000を計算し、1200000000を出力する。
  ✓ 3 - 4を計算し、-1を出力する
  ✓ 653180531098475310 - 780320895328957を計算し、652400210203146400を出力する (1 ms)
  ✓ 300 * 1.23を計算し、369を出力する
  ✓ 100 * 0.53を計算し、53を出力する
  ✓ 4 / 3を計算し、1.33333を出力する (1 ms)
  ✓ 5 / 2を計算し、2.5を出力する (1 ms)
  ✓ 2 ^ 3を計算し、8を出力する (1 ms)
  ✓ 99 ^ 3289を計算し、90438207500880450000を出力する

Test Suites: 1 passed, 1 total
Tests:       10 passed, 10 total
Snapshots:   0 total
Time:        0.454 s, estimated 1 s
Ran all test suites.
✨  Done in 1.28s.
```

- テスト実行ができた！