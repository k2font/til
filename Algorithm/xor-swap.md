# 一時変数を用意せずに、2つの変数の値を交換する
### 概要
- 方法は2つある
- 1つ目は単純な足し算と引き算を用いる方法。ただしこの方法は値が大きな場合にオーバーフローを引き起こす場合がある
- 2つ目はXORを用いる方法。

### 単純な足し算と引き算を用いる方法
- 足し算と引き算のみで、一時変数を用いること無くswapが実現できる

```cpp
int x = 1; int y = 2;
cout << x << " " << y << endl; // 1 2
x = x + y; // x = 3となる
y = x - y; // 3 - 2よりy = 1となる
x = x - y; // 3 - 1よりx = 2となる
cout << x << " " << y << endl; // 2 1
```

### XORを用いる方法
- `x ⊕ y`を`x`として、さらに`y = x ⊕ y`、`x = x ⊕ y`とすることでswapが実現できる
- よく考えると当然のこと。以下を考えるとわかる

```cpp
x = x ⊕ y // 前処理。
y = x ⊕ y ⊕ y // y = x ⊕ y をする時、x = x ⊕ yなのでこのようになる。
// なお、y ⊕ y = 0なので、この式はy = xとなる。

x = x ⊕ x ⊕ y // この時点でy = xであり、x = x ⊕ yなのでこのようになる。
// 同様にx ⊕ x = 0のため、x = yとなる。

// これでx = 2、y = 1となりswap成功！
```

```cpp
int x = 1; int y = 2;
cout << x << " " << y << endl; // 1 2
x = x ^ y; // x = 3となる
y = x ^ y; // 3 ⊕ 2よりy = 1となる
x = x ^ y; // 3 ⊕ 1よりx = 2となる
cout << x << " " << y << endl; // 2 1
```

### 注意点
- XORを用いる方法は、現代のコンピュータとしては効率的とは言えない。素直に`swap()`を用いたほうがいい。
- 理由はCPUの動作原理にある。
  - 参考: https://ja.wikipedia.org/wiki/%E3%83%AC%E3%82%B8%E3%82%B9%E3%82%BF%E3%83%BB%E3%83%AA%E3%83%8D%E3%83%BC%E3%83%9F%E3%83%B3%E3%82%B0
- たとえばアセンブリであれば本アルゴリズムを用いたスワップは大いに有効である。