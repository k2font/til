# Union Find
### 概要
- 集合を木で管理するデータ構造
- 次の事ができる
  - あるグラフの頂点(もしくは単一の頂点)を他のグラフの頂点に連結する
  - ある頂点がUnionFind木に含まれているかどうかを判定する
  - UnionFind木のサイズ(木に含まれている頂点数)を計算する
- 次のことが苦手
  - UnionFind木に含まれる頂点A, Bの連結を解除する

### こんなことができる
- あるグラフの橋(グラフとグラフを結んでいる唯一の辺。その辺を取り払うと非連結なグラフが2つ生まれてしまうような辺)を求めたい時
- 辺の重さが最小となる木を構成したい時 👉 [クラスカル法](/Algorithm/Kruskal.md)

### サンプルコード
- 以下の構造体がUnionFindの各種操作を行う。
- UnionFindに関しては、使い方さえ理解できれば本番はコピペでよい。

```cpp
// UnionFindを示す構造体
struct UnionFind {
    vector<int> par;
    
    UnionFind(int n) : par(n, -1) { }

    int root(int x) {
        if (par[x] < 0) return x;
        else return par[x] = root(par[x]);
    }
    
    bool issame(int x, int y) {
        return root(x) == root(y);
    }
    
    bool merge(int x, int y) {
        x = root(x); y = root(y);
        if (x == y) return false;
        if (par[x] > par[y]) swap(x, y); // merge technique
        par[x] += par[y];
        par[y] = x;
        return true;
    }
    
    int size(int x) {
        return -par[root(x)];
    }
};
```

- 使い方

```cpp
int main() {
  int N, Q; cin >> N >> Q;
  vector<int> P(Q), A(Q), B(Q);
  REP(i, Q) cin >> P[i] >> A[i] >> B[i];
  UnionFind uf(N);

  REP(i, Q) {
    if(P[i] == 0) {
      uf.merge(A[i], B[i]); // グラフの結合はuniteで簡単
    } else {
      if(uf.issame(A[i], B[i])) cout << "Yes" << endl; // AとBが同じグラフにいるかどうかはsameで簡単
      else cout << "No" << endl;
    }
  }
}
```

- `merge(A, B)`: 頂点AとBを連結にする
- `issame(A, B)`: 頂点AとBが同じ木上に存在するかを判定する
- `size(A)`: 頂点Aが含まれる木の頂点数を計算する