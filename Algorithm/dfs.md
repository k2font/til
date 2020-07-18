# 深さ優先探索(DFS)
### 概要
- 始点から(木の場合は根から)最も深いところまで掘っていく探索法。
- グラフ・木の探索から複雑な条件下の全探索まで、応用範囲が広い。書いていて楽しいアルゴリズムの1つ。

#### 用途1: グラフの探索
- グラフの連結成分の個数を数えるときに活用できる
- グラフの地点AからBへの最短距離は[BFS](/Algorithm/bfs.md)を使用する

#### 用途2: 木の探索
- ほとんどの木の探索問題はDFSが有効利用できる
- 木を見たら、まずはDFSの使用を検討する。
  - UnionFindや最小全域木の可能性もあるので、あまり深入りしないように気をつける

#### 用途3: 全探索
- 複雑な条件下の全パターンを探索することにも使用できる
- ...というより、樹形図で書き表せるものの探索に適していると考える。これは上記の木の探索と全く同じ。

### サンプルコード

#### グラフや木の探索
```cpp
#include <bits/stdc++.h>
using namespace std;
using Graph = vector<vector<int>>;

// 深さ優先探索
vector<bool> seen;
void dfs(const Graph &G, int v) {
    seen[v] = true; // v を訪問済にする

    // v から行ける各頂点 next_v について
    for (auto next_v : G[v]) { 
        if (seen[next_v]) continue; // next_v が探索済だったらスルー
        dfs(G, next_v); // 再帰的に探索
    }
}

int main() {
    // 頂点数と辺数
    int N, M; cin >> N >> M;

    // グラフ入力受取 (ここでは無向グラフを想定)
    Graph G(N);
    for (int i = 0; i < M; ++i) {
        int a, b;
        cin >> a >> b;
        G[a].push_back(b);
        G[b].push_back(a);
    }

    // 頂点 0 をスタートとした探索
    seen.assign(N, false); // 全頂点を「未訪問」に初期化
    dfs(G, 0);
}
```

#### 全探索
```cpp
#include <bits/stdc++.h>
using namespace std;

int N, M, Q;
vector<int> a, b, c, d;
ll ans = 0;
void dfs(vector<int> &A, int tmp) {
  if(A.size() == N) {
    ll tmp = 0;
    REP(i, Q) {
      if(A[b[i] - 1] - A[a[i] - 1] == c[i]) {
        tmp += d[i];
      }
    }
    if(ans < tmp) ans = tmp;
    return;
  }

  for(int i = tmp; i <= M; ++i) {
    A.push_back(i);
    dfs(A, i);
    A.pop_back();
  }
}

int main() {
  cin >> N >> M >> Q;
  a.resize(Q); b.resize(Q); c.resize(Q); d.resize(Q);
  REP(i, Q) cin >> a[i] >> b[i] >> c[i] >> d[i];
  vector<int> A;
  dfs(A, 1);
  cout << ans << endl;
}
```