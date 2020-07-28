# 条件付きレンダリング
### 概要
- React(というよりJSX)は、アプリケーションの状態に応じて異なるコンポーネントをレンダリングすることができる
- 以下のような例がわかりやすい。

```typescript
import React from 'react';

interface Props {
  isLoggedIn?: boolean;
}

function UserGreeting(props: Props) {
  return <h1>Welcome back!</h1>;
}

function GuessGreeting(props: Props) {
  return <h1>Please sign up.</h1>;
}

// props.isLoggedInの値を見て表示するコンポーネントを変更する
function Greeting(props: Props) {
  const isLoggedIn = props.isLoggedIn;
  if(isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuessGreeting />;
}

export default function App() {
  return (
    <div className="App">
      <header className="App-header">
        <Greeting isLoggedIn={false} />
      </header>
    </div>
  );
}
```

### 要素変数を用いてレンダリングする要素を変更する
- 要素を格納しておく変数に何を入れるかを、`this.state`の値を見て変化させる。

```typescript
import React from 'react';

interface Props {
  isLoggedIn?: boolean;
  onClick?: any;
}

interface IState {
  isLoggedIn?: boolean;
}

function UserGreeting(props: Props) {
  return <h1>Welcome back!</h1>;
}

function GuessGreeting(props: Props) {
  return <h1>Please sign up.</h1>;
}

function Greeting(props: Props) {
  const isLoggedIn = props.isLoggedIn;
  if(isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuessGreeting />;
}

function LoginButton(props: Props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props: Props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}

class LoginControl extends React.Component<Props, IState> {
  constructor(props: Props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {
      isLoggedIn: true
    };
  }

  // ボタンが押されたらisLoggedInにtrueを格納する
  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  // ボタンが押されたらisLoggedInにfalseを格納する
  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn: any = this.state.isLoggedIn;
    let button: any;
    // 以下が要素変数に何を入れるかを決定している
    if(isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }
    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

export default function App() {
  return (
    <div className="App">
      <header className="App-header">
        <LoginControl />
      </header>
    </div>
  );
}
```

- こんな感じになる。いい感じ!

[![Image from Gyazo](https://i.gyazo.com/476b1cc42cd118b9192b3342a002d9f4.gif)](https://gyazo.com/476b1cc42cd118b9192b3342a002d9f4)

### 論理演算子を用いた要素の変更
- 論理演算子 `&&` を用いた要素の出し分けを行うことが可能
- `true && expression`は必ず`expression`と評価され、`false && expression`であれば必ず`expression`と評価される。そのため、このような表記が有効な場合がある。

```typescript
import React from 'react';

interface Message {
  unreadMessages: string[];
}
 
function Mailbox(props: Message) {
  const unreadMessages: string[] = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      // この部分に用いている
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages: string[] = ['React', 'Re: React', 'Re:Re: React'];
export default function App() {
  return (
    <div className="App">
      <header className="App-header">
        <Mailbox unreadMessages={messages} />
      </header>
    </div>
  );
}
```

- `message`の値が空であれば「You have ...」から始まる要素は表示されない。

<img width="661" alt="スクリーンショット 2020-07-28 16 44 24" src="https://user-images.githubusercontent.com/6561417/88634708-b32d2f00-d0f1-11ea-9f86-12adb0698c36.png">

### 三項演算子を用いた要素の変更
- 三項演算子`condition ? true : false`を使うことができる
- ただし見づらくなる可能性があるので、採用する書き方は開発チームの方針に沿って選定してね

```typescript
// コードの一部
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```