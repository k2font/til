# `--isolatedModules`オプションを付けて`react-scripts start`するときの注意点
### 結論
- すべての`.ts`ファイルがモジュールになるため、何らかのオブジェクトをexportする必要がある。
- これは`tsconfig.json`で`"isolatedModules": true`としている場合も同様の処理が必要となる。

### どういうこと？
- `react-scripts start`時にエラーが出る例
```typescript
function extend<T, U>(first: T, second: U): T & U { // error!
  return { ...first, ...second };
}

const x = extend({ a: "hello" }, { b: "shoichiro" });
```

- `All files must be modules when the '--isolatedModules' flag is provided. ts(xxxx)`と出力されうまく動かない。
- この時、何らかのオブジェクトを`export`すればエラーが消える。
  - これは、すべてのファイルがモジュールとして認識されているためである。

```typescript
export function extend<T, U>(first: T, second: U): T & U {
  return { ...first, ...second };
}

const x = extend({ a: "hello" }, { b: "shoichiro" });
```

### `--isolatedModules`って何？
- `tsc`のコンパイラオプションの1つ。
- 公式ドキュメントには`Perform additional checks to ensure that separate compilation (such as with transpileModule or @babel/plugin-transform-typescript) would be safe.`とある。
  - つまり、すべての`.ts`ファイルをモジュールに見立てて、それらを個別にコンパイルし、安全であるかどうかを確認する処理が走るようになる。
  - モジュールであるにもかかわらず、`.ts`ファイルが何もexportしない状態となっているため、tscはエラーを出力する。