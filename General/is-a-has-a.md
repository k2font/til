# is-a関係とhas-a関係
## is-a関係
- is = 〜は である通り、分類を表す
- 人間は動物、猫は動物、みたいな感じ
  - つまりプログラミングにおいてはクラスの継承によって表現される
- つまりたくさんのオブジェクトをグルーピングするのに使う関係、それがis-a関係

```ts
class Animal {
  eat: void {
    console.log('ガツガツ');
  }
}

class Dog extends Animal {
  cry: void {
    console.log('ワンワン');
  }
}

class Cat extends Animal {
  cry: void {
    console.log('ニャー');
  }
}

const dog = new Dog();
const cat = new Cat();
dog.eat(); // ガツガツ
dog.cry(); // ワンワン
cat.eat(); // ガツガツ
cat.cry(); // ニャー
```

## has-a関係
- has = 持っている である通り、機能を持っている状態を表す
- プログラミングにおいては、mixinやimplementsを使って表現する

```ts
class Animal {
  eat: void {
    console.log('ガツガツ');
  }
}

// mixin
class Dog {
  animal: Animal;
  constructor(animal: Animal) {
    this.animal = animal;
  }
  cry: void {
    console.log('ワンワン');
  }
}
```
