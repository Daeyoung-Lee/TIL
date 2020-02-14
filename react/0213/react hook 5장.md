# react hook 5장

2020-02-13

> 함수형 컴포넌트에서도 컴포넌트의 상태값을 관리할 수 있고,
>
> 컴포넌트의 생명 주기 함수를 이용할 수 있음
>
> 함수형 컴포넌트에서 클래스형 컴포넌트를 쓸 수 있도록 도와주는 기술
>
> (상태변수와 생명 주기 사용이 안 됐던 것을 되게 해줌)



## p217 React.useState 훅



```javascript
/*
import React from 'react';

class Profile extends React.Component {
    state = { name: '' };
    onChange = e => {this.setState({ name: e.target.value }) };
    render() {
        return (
            <>
                <p>{`My name is ${this.state.name}`}</p>
                <p><input type="text" value={this.state.name} onChange={this.onChange}/></p>
            </>
        )
    }
}

export default Profile;
*/

import React, {useState} from 'react'

function Profile () {
    //state = { name: '' };
    // [ 상태값, 상태값변경함수 ] = React.userState(초기값);
    const [ name, setName ] = useState('');
    return (
        <>
            <p>{`My name is ${name}`}</p>
            <p><input type="text" value={name} onChange={e => setName(e.target.value)}/></p>
        </>
    );
}

export default Profile;

```



### 1방법

```javascript
/*
import React from 'react';

class Profile extends React.Component {
    state = { name: '', age: 0 };
    onChangeName = e => {this.setState({ name: e.target.value }) };
    onChangeAge = e => {this.setState({ age: e.target.value }) };
    render() {
        return (
            <>
                <p>{`이름: ${this.state.name}`}</p>
                <p>{`나이: ${this.state.age}`}</p>
                <p><input type="text" value={this.state.name} onChange={this.onChangeName}/></p>
                <p><input type="text" value={this.state.age} onChange={this.onChangeAge}/></p>
            </>
        )
    }
}

export default Profile;
*/


import React, {useState} from 'react'

function Profile () {
    //state = { name: '' };
    // [ 상태값, 상태값변경함수 ] = React.userState(초기값);
    const [ name, setName ] = useState('');
    const [ age, setAge ] = useState('');
    
    return (
        <>
            <p>{`이름: ${name}`}</p>
            <p>{`나이: ${age}`}</p>
            <p><input type="text" value={name} onChange={e => setName(e.target.value)}/></p>
            <p><input type="text" value={age} onChange={e => setAge(e.target.value)}/></p>
        </>
    );
}

export default Profile;

```



### 2방법

```javascript
import React, {useState} from 'react'

function Profile () {
    //state = { name: '', age: 0 };
    // [ 상태값, 상태값변경함수 ] = React.userState(초기값);
    const [ state, setState ] = useState({ name: '', age: 0 });
        
    return (
        <>
            <p>{`이름: ${state.name}`}</p>
            <p>{`나이: ${state.age}`}</p>
            <p><input type="text" value={state.name} onChange={e => setState({ ...state, name: e.target.value })}/></p>
            <p><input type="text" value={state.age} onChange={e => setState({...state, age:e.target.value})}/></p>
        </>
    );
}

export default Profile;

```



## p220 React.useEffect 훅

생명주기 함수를 이용해서 DOM 이외에 상태변수의 값을 출력

useEffect 훅을 사용하면 코드 중복을 줄이고 간결하게 만들 수 있다.

```javascript
/*
import React from 'react';

class Profile extends React.Component {
    state = { count: 0 };
    onClick = () => {
        this.setState({ count: this.state.count + 1 });
    };
    // 클래스형 컴포넌트에서 생명주기 함수를 이용하면 부득이 코드 중복이 발생할 수 있다.
    componentDidMount() {
        document.title = `현재 클릭 수 : ${this.state.count}`;
    }
    componentDidUpdate() {
        document.title = `현재 클릭 수 : ${this.state.count}`;
    }

    render() {
        return(
            <div>
                <p>현재 클릭 수: {this.state.count}</p>
                <p><button onClick={this.onClick}>클릭</button></p>
            </div>
        );
    }
}

export default Profile;
*/

import React, { useEffect, useState } from 'react';

function Profile () {
    const [ count, setCount ] = useState(0);
    // 렌더링 결과가 실제 돔에 반영된 후 호출되는 훅
    useEffect(() => {
        document.title = `현재 클릭 수 : ${count}`;
    });

    return(
        <div>
            <p>현재 클릭 수: {count}</p>
            <p><button onClick={() => setCount(count +1)}>클릭</button></p>
        </div>
    );
}
export default Profile;

```



## p222 이벤트 처리 함수를 등록하고 해제하기

생명주기 함수를 이용하면 등록과 해제가 분리되어 코드 누락이 발생할 수 있다.



```javascript
/*
import React from 'react';

class Profile extends React.Component {
    state = { width: window.innerWidth };

    onResize = () => {
        this.setState({ width: window.innerWidth });
    };
    // resize 이벤트 리스너를 등록

    componentDidMount() {
        window.addEventListener('resize', this.onResize);
    };

    // resize 이벤트 리스너를 해제
    componentWillUnmount() {
        window.removeEventListener('resize', this.onResize);
    }

    render() {
        return<div>{`width is ${this.state.width}`}</div>
        ;
    }
}

export default Profile;
*/


import React, { useEffect, useState } from 'react';

function Profile () {
    const [ width, setWidth ] = useState(window.innerWidth);
    useEffect(() => {
        const onResize = () => setWidth(window.innerWidth);
        window.addEventListener('resize', onResize);

        return () => window.removeEventListener('resize', onResize);
    }, []);
        return <div>{`width is ${width}`}</div>;

}
export default Profile;

```



## react router

설치 npm install react-router-dom 

샘플

```javascript
import React from "react";
import {
    BrowserRouter as Router,
    Switch,
    Route,
    Link
} from "react-router-dom";
import Todo from './Todo';
export default function App() {
    return (
        <Router>
            <div>
                <Link to="/">Home</Link>
                | <Link to="/about">About</Link>
                | <Link to="/users">Users</Link>
                | <Link to="/todo">할일목록</Link>

                <Switch>
                    <Route path="/about"><About /></Route>
                    <Route path="/users"><Users /></Route>
                    <Route path="/todo"><Todo /></Route>
                    <Route path="/"><Home /></Route>
                </Switch>
            </div>
        </Router>
    );
}

function Home() {
    return <h2>Home</h2>;
}

function About() {
    return <h2>About</h2>;
}

function Users() {
    return <h2>Users</h2>;
}

```



## P221 생명주기 함수에서 API를 호출하는 경우

```javascript
// MyComponent.js : 클래스형 컴포넌트 
import React from 'react';

class MyComponent extends React.Component {
    state = { user: null };
    //  최초 렌더링 시에 API 호출 
    componentDidMount() {
        const { userId } = this.props;
        getUserApi(userId).then(data => this.setState({ user: data }));
    }
    //  이후 렌더링 시에 API 호출 → 동일한 기능을 중복해서 작성
    componentDidUpdate(prevProps) {
        const { userId } = this.props;
        if (userId !== prevProps.userId) {
            getUserApi(userId).then(data => this.setState({ user: data }));
        }
    }
    render() {
        const { user } = this.state;
        return (
            <div>
                { !user && <p>사용자 정보를 가져오는 중 ...</p> }
                { user && (
                    <>
                        <p>{`name is ${user.name}`}</p>
                        <p>{`age is ${user.age}`}</p>
                    </>
                ) }
            </div>
        );
    }
}
function getUserApi(userId) {
    return Promise.resolve({userId: 123, name: '홍길동', age: 23});
}

export default MyComponent;

```

### hook으로

```javascript
// 훅으로 구현
import React, { useState, useEffect } from 'react';

function MyComponent ({userId}) {
    const [ user, setUser ] = useState(null);
    useEffect(() => { 
        getUserApi(userId).then(data => setUser(data))
    });
    return (
        <div>
            { !user && <p>사용자 정보를 가져오는 중 ...</p> }
            { user && (
                <>
                    <p>{`name is ${user.name}`}</p>
                    <p>{`age is ${user.age}`}</p>
                </>
            ) }
        </div>
    );

}
function getUserApi(userId) {
    return Promise.resolve({userId: 123, name: '홍길동', age: 23});
}

export default MyComponent;

```



## 예제1

```javascript
//  Profile.js
import React from 'react';
 
class Profile extends React.Component {
    state = { user: null, width: window.innerWidth };
    onResize = () => {
        this.setState({ width: window.innerWidth });
    }
    componentDidMount() {
        const { userId } = this.props;
        getUserApi(userId).then(data => this.setState({ user: data }));
        window.addEventListener('resize', this.onResize);
    }
    componentWillUnmount() {
        window.removeEventListener('resize', this.onResize);
    }
    componentDidUpdate(prevProps) {
        const { userId } = this.props;
        if (userId !== prevProps.userId) {
            getUserApi(userId).then(data => this.setState({ user: data }));
        }
    }
    render() {
        const { user, width } = this.state;
        return (
            <div>
                { !user && <p>사용자 정보를 조회 중 입니다.</p> }
                { user && (
                    <>
                        <p>{`user name is ${user.name}`}</p>
                        <p>{`user age is ${user.age}`}</p>
                    </> 
                )}
                <p>{`window width is ${width}`}</p>
            </div>
        );
    }
}
function getUserApi(userId) {
    return Promise.resolve({userId: 123, name: '홍길동', age: 22});
}
export default Profile;

```

### hook으로

```javascript
// 함수형 컴포넌트로 전환
//  Profile.js
import React, { useEffect, useState } from 'react';
 
function Profile ({ userId}) {
    const [ user, setUser ] = useState(null);
    const [ width, setWidth ] = useState(window.innerWidth);

    useEffect(
        () => {
            getUserApi(userId).then(data => setUser(data));
        }, 
        [ userId ]
    );
    useEffect(
        () => { 
            const onResize = () => setWidth(window.innerWidth);
            window.addEventListener('resize', onResize);
            return () => window.removeEventListener('resize', onResize);
        }
    );
    return (
        <div>
            { !user && <p>사용자 정보를 조회 중 입니다.</p> }
            { user && (
                <>
                    <p>{`user name is ${user.name}`}</p>
                    <p>{`user age is ${user.age}`}</p>
                </> 
            )}
            <p>{`window width is ${width}`}</p>
        </div>
    );

}
function getUserApi(userId) {
    return Promise.resolve({userId: 123, name: '홍길동', age: 22});
}
export default Profile;

```



## useWindowWidth.js

```javascript
import { useEffect, useState } from 'react'

function useWindowWidth() {
    const [ width, setWidth ] = useState(window.innerWidth);

    useEffect(
        () => {
            const onResize = () => { setWidth(window.innerWidth); };
            window.addEventListener('resize', onResize);
            return () => window.removeEventListener('resize', onResize);
        }
    );
        return width;
}

export default useWindowWidth;

```



## Profile.js

```javascript
import React, { useState } from 'react';
import useWindowWidth from './useWindowWidth';

function Profile ({ userId}) {
    const width = useWindowWidth();
    const [ name, setName ] = useState('');
    return (
        <div>
            <p>{`name is ${name}`}</p>
            <p>{`width is ${width}`}</p>
            { width < 600 && <br/> }
            <input type="text" value={name} onChange={e => setName(e.target.value)} />
        </div>
    );
}
export default Profile;

```



## useHasMounted.js

```javascript
import { useState, useEffect } from 'react';

function useHasMounted() {
    const [ hasMounted, setHasMounted ] = useState(false);
    useEffect(
        () => setHasMounted(true)   
    );
    return hasMounted;
}

```



## p230 useContext 훅

```javascript
import React, { useContext } from 'react';

const UserContent = React.createContext();

function Profile () {
    const user = { name: '홍길동', age: 23 };
    return (
        <div>
            <UserContent.Provider value={user}>
                <ParentComponent/>
            </UserContent.Provider>
        </div>
    );
}

function ParentComponent() {
    return (
        <div>
            <ChildComponent />
            <ChildComponentWithHook />
        </div>
    );
}

function ChildComponent() {
    return (
        <UserContent.Consumer>
            { user => (
                <>
                <p>{`name is ${user.name}`}</p>
                <p>{`age is ${user.age}`}</p>
                </>
            )}
        </UserContent.Consumer>
    );
}

function ChildComponentWithHook() {
    const user = useContext(UserContent);
    return (
        <>
        <p>{`name is ${user.name}`}</p>
        <p>{`age is ${user.age}`}</p>
        </>
    );
}
export default Profile;

```



## p231 useRef 훅

```javascript
// 클래스형 컴포넌트
import React from 'react';

class Profile extends React.Component {
    inputEl1 = React.createRef();
    inputEl2 = React.createRef();
    onClick = () => {
        if (this.inputEl1.current) {
            this.inputEl1.current.focus();
        }
    };
    componentDidMount() {
        if (this.inputEl2.current) {
            this.inputEl2.current.focus();
        }
    };
    render() {
        return(
            <div>
                <input ref={this.inputEl1} type="text" value="inputEl1" />
                <input ref={this.inputEl2} type="text" value="inputEl2" />
                <button onClick={this.onClick}>inputE1으로 포커스 이동</button>
            </div>
        );
    };
}

export default Profile;


// 함수형 컴포넌트
import React, { useEffect } from 'react';

function Profile () {
    const inputEl1 = React.useRef();
    const inputEl2 = React.useRef();
    const onClick = () => {
        if (inputEl1.current) {
            inputEl1.current.focus();
        }
    };
    useEffect(() => {
        if (inputEl2.current) {
            inputEl2.current.focus();
        }
    });
    return(
        <div>
            <input ref={inputEl1} type="text" value="inputEl1" />
            <input ref={inputEl2} type="text" value="inputEl2" />
            <button onClick={onClick}>inputE1으로 포커스 이동</button>
        </div>
    );

}

export default Profile;

```















