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

































