# matcherの利用
- matcherは、`expect`が発行した`expectation`オブジェクトの評価を行うもの。
- `toBe`や`toBeCloseTo`がそれにあたる。

### 文字列の評価
- `toMatch()`と[正規表現]()を用いてマッチするかどうかを評価する。
- マッチしないものを期待する場合は`not`を挟むと良い。
```javascript
// 文字列がマッチするかどうかは正規表現を用いる。
test('iamnotshoichiroという文字列にpが入っていないか', () => {
  expect("iamnotshoichiro").not.toMatch(/p/);
});
```

### 数値の評価
- 数値の評価には様々なmatcherを用いることができる。
- `=`による完全一致だけでなく、`<`や`≧`などの不等号のような考え方を用いて値の評価を行うことも可能である。

```javascript
test('2 + 2を様々な方法で確認する', () => {
  const value = 2 + 2;
  expect(value).toBeGreaterThan(3); // >
  expect(value).toBeGreaterThanOrEqual(3.5); // ≧
  expect(value).toBeLessThan(5); // <
  expect(value).toBeLessThanOrEqual(4.5); // ≦

  // 数値型においてtoBeとtoEqualは同じ挙動を見せる。
  expect(value).toBe(4); // =
  expect(value).toEqual(4); // =
});
```

- 浮動小数点の評価の場合、たとえば内部的には`0.1 + 0.2 = 0.300000000000000004`と誤差が生じる場合がある。
  - この時、`toBe`で正しく評価をすることが出来ない。
  - `toBeCloseTo`を用いることで誤差にとらわれない評価を行うことができる。

```javascript
test('浮動小数点数の評価', () => {
  const value = 0.1 + 0.2;
  expect(value).toBeCloseTo(0.3); // 浮動小数点数はtoBeCloseToで比較する
})
```

### nullとundefined

- `toBeNull`や`toBeUndefined`など、`null`や`undefined`を評価するmatcherが用意されている

```javascript
test('nullかどうかを試験する。', () => {
  const n = null;
  expect(n).toBeNull();
  expect(n).toBeDefined();
  expect(n).not.toBeUndefined();
  expect(n).not.toBeTruthy();
  expect(n).toBeFalsy();
});
```

### 0の評価

- [nullとundefined](#nullとundefined)と同様のmatcherを使用できる

```javascript
test('0かどうかを試験する。', () => {
  const z = 0;
  expect(z).not.toBeNull();
  expect(z).toBeDefined();
  expect(z).not.toBeUndefined();
  expect(z).not.toBeTruthy();
  expect(z).toBeFalsy();
});
```

### 反復可能なオブジェクトの評価

- `toContain`を用いて配列に値が含まれているかどうかの評価を行う。

```javascript
const shoppingList = [
  'diapers',
  'kleenex',
  'trash bags',
  'paper towels',
  'beer',
];

test('shoppingListの中に指定した値が含まれているかどうか', () => {
  expect(shoppingList).toContain('beer');
  expect(new Set(shoppingList)).toContain('beer');
})
```