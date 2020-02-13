## Parent.js

```javascript
import React from 'react'
import Child from './Child'

class Parent extends React.Component {
    state = {
        name: ''
    };
    
    onChange = e => {
        let { name } = this.state;
        name = e.target.value;
        this.setState = ({ name });
    };

    render() {
        return (
            <>
            <h1>My name is {this.state.name}</h1>
            <Child name={this.state.name} onChange={this.onChange}/>
            </>
        );
    }
}

export default Parent;

```





## Child.js

```javascript
import React from 'react';

function Child ({ name, onChange }) {
    return(
        <div>
            <input type ="text" value = {name} onChange={onChange} />
        </div>
    );
}

export default Child;

```









































## TodoItemList.js



```javascript
import React, { Component } from 'react';
import TodoItem from './TodoItem';
import { MdCheckBoxOutlineBlank, MdCheckBox, MdRemoveCircleOutline, } from 'react-icons/md';
import cn from 'classnames';
import './TodoItem.css';

class TodoItemList extends Component {
  render() {
    const { todos, onToggle, onRemove } = this.props;

    const todoList = todos.map(
      ({id, text, checked}) => (
        <TodoItem
          id={id}
          text={text}
          checked={checked}
          onToggle={onToggle}
          onRemove={onRemove}
          key={id}
        />
      )
    );

    return (
      <div>
        {todoList}    
      </div>
    );
  }
}

export default TodoItemList;

```

