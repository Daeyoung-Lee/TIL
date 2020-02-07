# react

4일차npm start



## p111, React.memo, React.PureComponent

자식 컴포넌트는 부모 컴포넌트가 렌더링될 때 함께 렌더링된다.

`증가`, `증가2` 에서

`증가2`를 눌렀을 때 자식 컴포넌트도 같이 렌더링 됨

=> 불필요한 렌더링이 발생

=> 불필요한 렌더링을 방지하기 위해서는 React.memo, React.PureComponent를 사용



### APP.js

```javascript
import React from 'react';
import Todo from './Todo';

class App extends React.Component {
  render() {
    return <Todo />
  }
}

export default App;

```



### Todo.js

```javascript
import React, { Fragment } from 'react';
import Title from './Title';

class Todo extends React.Component {
    state = { count: 0, count1: 0 };

    onClick = () => {
        this.setState({ count: this.state.count +1});
    };
    
    onClick2 = () => {
        this.setState({ count1: this.state.count1 +1});
    };

    render() {
        return (
            <div>
                <Title title = {`현재 카운트: ${this.state.count}`}></Title>
                <p>{this.state.count1}</p>
                <button onClick={this.onClick}>증가</button>
                <button onClick={this.onClick2}>증가2</button>
            </div>
        )
    }
}
export default Todo;

```



### Title.js

```javascript
import React from 'react';

function Title(props) {
    console.log(props);
    return <p>{props.title}</p>
}

export default Title;

```

---

### 불필요한 렌더링을 방지

### React.memo

`함수형 컴포넌트`인 경우에는 React.memo()를 이용해서 자식 컴포넌트의 불필요한 렌더링을 줄일 수 있음

#### Title.js

`export default React.memo(Title);`추가

```javascript
import React from 'react';

function Title(props) {
    console.log(props);
    return <p>{props.title}</p>
}

export default React.memo(Title);
// 함수형 컴포넌트인 경우에는 React.memo()를 이용해서 자식 컴포넌트의 불필요한 렌더링을 줄일 수 있음
// props값이 변경되는 경우에만 호출되는 것을 확인할 수 있음
```

`증가2`를 눌러도 자식 컴포넌트는 렌더링이 안됨



### React.PureComponent

`클래스형 컴포넌트`인 경우에는 React.PureComponent를 이용하면 자식 컴포넌트의 불필요한 렌더링을 줄일 수 있음

#### Title.js

```javascript
import React from 'react';

class Title extends React.PureComponent {
    constructor (props) {
        super(props);
    }
    
    render() {
        console.log(this.props);
        return <p>{this.props.title}</p>
    }
}

export default Title;

//클래스형 컴포넌트인 경우에는 React.PureComponent를 이용하면 자식 컴포넌트의 불필요한 렌더링을 줄일 수 있음
```



## p112, setState

* `클래스형 컴포넌트`에서 상태값을 변경할 때 호출하는 메소드
* setState 메서드로 입력된 객체는 기존 상탯값과 병합(merge)됨



### App.js

```javascript
import React from 'react';

class App extends React.Component {
  state = { count1: 0, count2: 0 };
  onClick = () => {
    this.setState({ count1: this.state.count1 + 1});
    this.setState({ count1: this.state.count1 + 1});
    this.setState({ count1: this.state.count1 + 1});
  };

  render() {
    const { count1, count2} = this.state;
    return (
      <div>
        <p>{count1}, {count2}</p>
          <button onClick={this.onClick}>증가</button>
      </div>
    );
  }
}

export default App;

```

3만큼 증가시키고 싶었는데 1만큼 증가한다.

이는 setState메서드가 비동기로 동작하기 때문이다.

리액트는 효율적으로 렌더링하기 위해 여러 개의 setState 메서드를 배치(batch)로 처리하기 때문이다.



### 방법1. 호출 직전의 상태값을 매개변수로 받아서 처리

#### App.js

```javascript
this.setState(prevState => ({ count2: prevState.count2 + 1 }));
```



```javascript
import React from 'react';

class App extends React.Component {
  state = { count1: 0, count2: 0 };
  onClick = () => {
    this.setState({ count1: this.state.count1 + 1});
    this.setState({ count1: this.state.count1 + 1});
    this.setState({ count1: this.state.count1 + 1});

    this.setState(prevState => ({ count2: prevState.count2 + 1 }));
    this.setState(prevState => ({ count2: prevState.count2 + 1 }));
    this.setState(prevState => ({ count2: prevState.count2 + 1 }));
  };

  render() {
    const { count1, count2} = this.state;
    return (
      <div>
        <p>{count1}, {count2}</p>
          <button onClick={this.onClick}>증가</button>
      </div>
    );
  }
}

export default App;

```



### 방법2. 상태값 로직을 분리해서 사용

#### App.js

```javascript
import React from 'react';

const actions = {
  init() {
    return { count: 0 };
  },

  increment(state) {
    return { count: state.count + 1 };
  },

  decrement(state) {
    return { count: state.count - 1 };
  },
};

class App extends React.Component {
  state = actions.init();

  onIncrement = () => {
    this.setState(actions.increment);
  };
  /* 
  this.setState(prevState => ({ count2: prevState.count + 1 }));
  이 함수를 밖에 actions로 빼 놓음, 따라서 위 increment의 state가 prevState
  */
  onDecrement = () => {
    this.setState(actions.decrement);
  };

  render() {
    return (
      <div>
        <p>{this.state.count}</p>
        <button onClick={this.onIncrement}>증가</button>
        <button onClick={this.onDecrement}>감소</button>

      </div>
    );
  }
}

export default App;

```



## p114 setState 메소드는 비동기로 처리되지만 호출 순서는 보장된다.

### App.js

```javascript
import React from 'react';

class App extends React.Component {
  state = { count1: 0, count2: 0 };

  onClick = () => {    
    this.setState({ count1: this.state.count1 + 1});
    this.setState({ count2: this.state.count2 + 1});
  };
/*
 onClick = () => {

  let { count1, count2 } = this.state;

  count1 += 1;

  count2 += 1;

  this.setState({ count1 });

  this.setState({ count2 });

};
*/
  render() {
    const { count1, count2} = this.state;
    let result = count1 >= count2;
    return (
      <div>
        <p>{count1} >= {count2}</p>
        <p>{String(result)}</p>
        <button onClick={this.onClick}>증가</button>
      </div>
    );
  }
}

export default App;

```



## p115 [setState 메소드의 두 번째 매개변수는 처리가 끝났을 때 호출되는 콜백 함수다.](https://ko.reactjs.org/docs/react-component.html#setstate)

두번째 매개변수는 `setState`의 실행이 완료되고 컴포넌트가 다시 렌더링된 뒤에 실행될 함수에 대한 콜백

 (componentDidUpdate()의 사용을 권장)

### App.js

```javascript
import React from 'react';

class App extends React.Component {
  state = { count1: 0, count2: 0 };

  onClick = () => {

    let { count1, count2 } = this.state;
  
    count1 += 1;
  
    count2 += 1;
  
    this.setState({ count1 }, () => console.log(`count1 = ${count1}`));
  
    this.setState({ count2 }, () => console.log(`${this.state.count2}`));
  }

  render() {
    const { count1, count2} = this.state;
    let result = count1 >= count2;
    console.log('render is called')
    return (
      <div>
        <p>{count1} >= {count2}</p>
        <p>{String(result)}</p>
        <button onClick={this.onClick}>증가</button>
      </div>
    );
  }
}

export default App;

```

`render is called` 가 찍히고 `count1`, `count2` 가 찍힌다.



## p116 3.2 리액트 요소와 가상 돔

JSX 코드에서 태그 사이에 표현식을 넣은 코드

```javascript
const element = <h1>제 나이는 {20 + 5} 세 입니다.</h1>;
가
{
    type: 'h1',
    props: {
        			// 아래처럼 표현식 앞, 뒤로 나뉘어서 들어감
        children: ['제 나이는', 25, '세 입니다.' ],
    }
}
```



### App.js



```javascript
import React from 'react';

class App extends React.Component {
  render() {
    const code3_16 = <a href="http://www.google.com">click here</a>;
    console.log(code3_16);
    const code3_17 = <a key="key1" style={{width:100}} href="http://google.com">click here</a>;
    console.log(code3_17);
    const code3_18 = <h1>제 나이는 {20+5} 세입니다.</h1>;
    console.log(code3_18);
    return(<div></div>);
  }
}

export default App;

```



### p119 리액트 요소가 돔 요소로 만들어지는 과정

리액트에서 데이터 변경에 의한 화면 업데이트는 `렌더 단계`와 `커밋 단계`를 거친다.

`렌더 단계`는 실제 돔에 반영할 변경 사항을 `파악`하는 단계,

`커밋 단계`는 변경 사함을 파악하기 위해 `가상 돔`을 이용

`가상 돔`은 리액트 요소로부터 만들어진다



#### App.js

```javascript
import React from 'react';
import Todo from './Todo';


class App extends React.Component {
  render() {
    const element = <Todo title="리액트 공부하기" desc="실전 리액트를 열심히 읽는다"></Todo>;
    console.log(element);
    return (element);
  }
}

export default App;

```



#### Todo.js

```javascript
import React, { Fragment } from 'react';
import Title from './Title';

class Todo extends React.Component {
    state = { priority: 'high'};

    onClick = () => {
        let { priority } = this.state;
        priority = priority === 'high' ? 'low' : 'high';
        this.setState({ priority });
    };

    render() {
        const { title, desc } = this.props;
        const { priority } = this.state;
        const element = (
            <div>
                <Title title={title} />
                <p>{desc}</p>
                <p>{priority === 'high' ? '우선순위 높음' : '우선순위 낮음'}</p>
                <button onClick={this.onClick}>우선순위 변경</button>
            </div>
        );
        console.log(element);
        return element;
    };
}

export default Todo;

```



#### Title.js

```javascript
import React from 'react';

class Title extends React.PureComponent {
    constructor (props) {
        super(props);
    }
    
    render() {
        const { title } = this.props;
        const element = <p style={{color: 'skyblue'}}>{title}</p>;
        console.log(element);
        return element;
    }
}

export default Title;

```



실제 돔을 만들 수 있는 리액트 요소 트리를 `가상 돔`이라고 한다.

최초의 리액트 요소 트리로부터 가상 돔을 만들고 이전 가상 돔과 비교해서 실제 돔에 반영할 내용을 결정하는 단계를 `렌더 단계`라고 부른다

브라우저에서 실제 돔을 변경하는 작업은 다른 작업에 비해 시간이 오래 걸리기 때문에 꼭 필요한 부분만 변경하는 것이 중요하다.



## p124 3.3 생명 주기 메서드















