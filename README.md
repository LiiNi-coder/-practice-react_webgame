# 1-3 첫 리액트 컴포넌트(아직은 Class)
## 컴포넌트 : 데이터 + 화면 + 자동 데이터 바인딩
기존 자바스크립트는 화면부터 생각해서 작성하고, 나중에 이 화면의 getElement를 하는 식으로 데이터바인딩을 수동으로 했다.
하지만, 리액트의 **컴포넌트**개념을 사용해, 데이터가 바뀌면 알아서 화면이 바뀌는 식이다.  
## 컴포넌트는 화살표함수(변수), 함수, 클래스 형태로 작성가능
단 클래스는 거의 안쓰이는데, Errorboundin할때만 쓰임
```javascript
import React, { useState } from 'react';

const ArrowFunctionComponent = () => {
  const [state, setState] = useState('Initial State');

  return (
    <div>
      <p>{state}</p>
      <button onClick={() => setState('State Changed')}>Change State</button>
    </div>
  );
};
```
```javascript
import React, { useState } from 'react';

function FunctionComponent() {
  const [state, setState] = useState('Initial State');

  return (
    <div>
      <p>{state}</p>
      <button onClick={() => setState('State Changed')}>Change State</button>
    </div>
  );
}
```
```javascript
import React from 'react';

class ClassComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { state: 'Initial State' };
  }

  render() {
    return (
      <div>
        <p>{this.state.state}</p>
        <button onClick={() => this.setState({ state: 'State Changed' })}>Change State</button>
      </div>
    );
  }
}
```

## 컴포넌트는 state와 render가 필수요소
### 화면에 바뀌는 부분을 state라고 생각하자
## 컴포넌트를 선언하고, 이를 렌더해야 html상에 나타난다.
```javascript
<script>
  'use strict';

  const e = React.createElement;

  class LikeButton extends React.Component {
    constructor(props) {
      super(props);
      this.state = {liked: false};
    }

    render() {
      if (this.state.liked) {
        return 'You liked this.';
      }

      return e('button', {onClick: () => this.setState({liked: true})}, 'Like');
    }
  }
</script>
<script>
  ReactDOM.render(e(LikeButton), document.querySelector('#root'));
</script>
```

# 1-4 가독성을 위한 JSX
## 위 방법의 react.createElement방법은 불편하니 jsx방법이 있음
아래는 각각의 컴포넌트 render함수의 리턴부분이다. 뭐가 다른지 보자
```javascript
return e('button', {onClick: () => this.setState({liked: true})}, 'Like');
...
<script>
  ReactDOM.render(e(클래스명), document.querySelector('#root'));
</script>
```
```javascript
return (
        <button onClick={() => this.setState({liked: true})}>
          Like
        </button>
      );
...
<script type="text/babel">
  ReactDOM.render(<클래스명/>, document.querySelector('#root'));
</script>
```
그런데 이렇게 작성하면 html파서가 <>문법을 알아먹질못함 그래서 babel을 사용
### 컴포넌트 이름은 반드시 대문자시작, jsx내부 html태그는 반드시 소문자시작
### JSX내부 html형식 내부에서 js코드를 적을땐 반드시 중괄호{}!
코드를 보면 obj는 2개의 중괄호인것을 유의
```javascript
return (
        <button onClick={() => this.setState({liked: true})} obj={{a:'b', c:'d'}}>
          Like {this.state.liked ? "Liked" : "Like!"}
          {[1, 2, 3].map((i)=>{
            return <div>i</div>;
          })}
        </button>
      );
```
### return할땐 형제태그 병렬 불가! 무조건 부모태그(열고닫고) 1개로만 이뤄져야함
아래는 **불가능 예제**
```javascript
return (
    <button onClick={()=>{this.setState({liked:true});}}>
    {this.state.liked? "Liked" : "Like!"}
    </button>
    <input type="text"/> //형제가 2명(button, input)이 나란히 return엔 불가능!
);
```
이렇게 div로 감싸면 가능!
```javascript
return (<div>
    <button onClick={()=>{this.setState({liked:true});}}>
    {this.state.liked? "Liked" : "Like!"}
    </button>
    <input type="text"/>
</div>
);
```
이렇게 fragment로 감싸는 것도 가능!
```javascript
return (<>
    <button onClick={()=>{this.setState({liked:true});}}>
    {this.state.liked? "Liked" : "Like!"}
    </button>
    <input type="text"/>
</>
);
```
## babel : jsx형식 -> 고전적 react(js기반)
html에서 script태그에 url을 줘서 포함시킬수있음
### babel을 html에서 태그로 사용할때 : `<script type="text/bael">`을 붙여서 명시해야함
## react18버전에서 가장 크게 달라진점 : 컴포넌트 렌더링함수[ReactDOM.render -> ReactDOM.createRoot]
```javascript
<script type="text/babel">
  //ReactDOM.render(<클래스명/>, document.querySelector('#root')); //react17버전
  ReactDOM.createRoot(document.querySelector("#root")).render(<클래스명 />); //react18버전
</script>
```
취직을 해서 이부분을 확인해서 어떤 버전인지 구분해야함
## react18에서 컴포넌트 렌더링할때, render에도 클래스이름태그를 여러개 적어서 여러개를 한번에 렌더링가능!
```javascript
<script type="text/babel">
  //ReactDOM.render(<클래스명/>, document.querySelector('#root')); //react17버전
  ReactDOM.createRoot(document.querySelector("#root")).render(
  <div>
    <클래스명 />
    <클래스명 />
    <클래스명 />
    <클래스명 />
    <클래스명 />
  </div>
  ); //react18버전
</script>
```
# 1-5 클래스 컴포넌트의 형태와 리액트 데브툴즈
## setState로 state를 변경해야한다
### 객체를 함부로 바꾸지마라(불변성), 객체는 복사해서 써라
```javascript
<button onClick={()=>{
    //this.state.liked = true; //잘못된예시
    setState({liked : true});
}}>
</button>
```
### 불변성 유지하는 배열객체메소드
- pop, push, shift, unshift, splice : 배열을 직접적으로 수정(불변성깨지니까 쓰면안됨)
- concat, slice : 새로운 배열 만들어냄
# 1-6 함수형 컴포넌트
## useState : 스테이트 선언 및 변경함수까지 네이밍
```javascript
<script type="text/babel">
  "use strict";
  funtion LikeButton(){
    const [liked, setLiked] = React.useState(false);
    if(liked){
      return "You Liked this";
    }
    return (
      <button onClick={()=>{setLiked(true);}}>Like</button>;
    );
  }
</script>
...
<script type="text/babel">ReactDOM.createRoot(document.querySelector("#root")).render(<LikeButton />);</script>
```
```javascript
<script type="text/babel">
  "use strict";
  const LikeButton = ()=>{
    const [liked, setLiked] = React.useState(false);
    if(liked){
      return "You Liked this";
    }
    return (
      <button onClick={()=>{setLiked(true);}}>Like</button>;
    );
  }
</script>
...
<script type="text/babel">ReactDOM.createRoot(document.querySelector("#root")).render(<LikeButton />);</script>
```
### 함수컴포넌트를 쓰면 this를 쓸필요가 없어서 좋다
# 1-7 구구단 리액트 만들기
1.구구단의 Gugudan.html에서 작업, 현재 이전 계산결과와 함께 정답유무를 같이 보여주도록 수정함.
## state란 변하는 모든것이다. gui효과도 state, 내부 글자바뀌는것도 state 죄다 state로 생각해야함!
### 왜 이 코드의 입력부분에 글씨를 입력할수없을까? (해답: input에 사용자가 글치는것도 state! => 관리필요)
```javascript
<script type="text/babel">
  class GuGuDan extends React.Component{
    constructor(props){
      super(props);
      //useState부분
      this.state = {
        first: Math.ceil(Math.random()*9),
        second: Math.ceil(Math.random()*9),
        value: '',
        result: '',
      };
    }

    render(){
      return (
        <div>
          <div>{this.state.first}곱하기{this.state.second}은?</div>
          <form>
            <input type="number" value={this.state.value}/>
            <button>입력!</button>
          </form>
          <div>{this.state.result}</div>
        </div>
      );
    }
  }
</script>
```
> input의 value가 리액트의 state값이라, 화면에 보여지는 input스트링은 무조건 state로 덮어씌워져있어서, 사용자가 아무리 입력을해도 state가 바뀌지 않기때문
**따라서 input의 onChange이벤트를 통해 state값을 갱신시켜줘야함!**
```javascript
//<input type="number" value={this.state.value})}}/>
<input type="number" value={this.state.value} onChange={(e)=>{this.setState({value: e.target.value})}}/>
```
