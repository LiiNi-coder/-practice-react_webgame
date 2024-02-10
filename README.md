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