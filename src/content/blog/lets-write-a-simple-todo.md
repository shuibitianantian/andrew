---
author: Andrew Xia
pubDatetime: 2024-02-15T12:34:26.400Z
modDatetime: 2024-02-15T12:34:26.400Z
title: Let's write a simple todo list
slug:  Let's write a simple todo list
featured: true
draft: false
tags:
  - docs
description: A simple todo list
---

## Write a simple todo list with React

> I believe every frontend developer was asked to train their react sense
> by developing such tiny application, another one is Movie DB

functions:

- [x]  Add task
- [x]  Delete task
- [x]  Display saved tasks

Nice to have: 

- [ ]  edit task
- [x]  finish task

Let’s see what does the app looks like?

![todo-2.png](@assets/images/todo-2.png)

We have an input to let user add new task and a list of saved tasks. As for each task, we can check the box at the front of each row to finish the task or we can delete the task from the todo list.

We can break down the app like this

![todo-1.png](@assets/images/todo-1.png)

Basically, this application has 3 parts, the task input allows user to input new task, the todo list view which displays all saved tasks and the todo item allows user to finish or delete the saved task. We can derive the components structure like this:

`TodoListApplication = TaskInput + TodoListView`

`TaskInput = input + add task button`

`TodoListView = TodoItem * N`

`TodoItem = finish checkbox + task name + delete button`

## What is “State” and how do we manage “State” of the application?

In Class Component, we use `this.state` and `this.setState` to achieve the state management. But in this tutorial, we simply focus on functional component. 

In Functional component, we use `useState` hook to achieve the same result.  (渲染过程其实就是一次函数调用)

Function Signature:

`function useState<S>(initialState: S | (() => S)): [S, Dispatch<SetStateAction<S>>];`

Example:

```tsx
...
const [text, setText] = useState(""); // the parameter is the initial state
<input value={text} onChange={(e) => setText(e.target.value)} />
...
// controlled component
```

> Any change of the state will trigger the rerendering of the component and its own children. 函数会被重新调用. State will only be initialized once.
> 

## Design of TodoItem

We have 2 functions on this component `delete task` and `finish task` , so the signature of this component should be like:

```tsx
interface IToDoItemProps {
    deleteTask: (id: string) => void;
    finishTask: (isFinished: boolean) => void;
}

// invoke
<ToDoItem 
    deleteTask={"a function to delete task"}
    finishTask={"a function to delete the task"}
/>

// Item.tsx
import { ToDoItem } from "../typings";

interface IItemProps {
  item: ToDoItem;
  deleteItem: () => void;
  finishItem: (isFinished: boolean) => void;
}

export default ({ item, deleteItem, finishItem }: IItemProps) => {
  return (
    <div
      style={{
        display: "flex",
        alignItems: "center",
        width: "300px",
        gap: 12,
      }}
    >
      <input
        type="checkbox"
        onChange={(v) => {
          finishItem(v.target.checked);
        }}
        className="todo-item"
      />
      <span
        style={{
          textDecoration: item.finished ? "line-through" : "",
          width: "200px",
        }}
      >
        {item.name}
      </span>
      <button className="button-4" onClick={deleteItem}>
        Delete
      </button>
    </div>
  );
};
```

## Design of TodoListView

This component simply display all the saved tasks, so the signature of this component should be like:

```tsx
// ListView.tsx
import { Dispatch, SetStateAction } from "react";
import { ToDoItem } from "../typings";
import Item from "./Item";

interface IListViewProps {
  todos: ToDoItem[];
  setTodos: Dispatch<SetStateAction<ToDoItem[]>>;
}

export default ({ todos, setTodos }: IListViewProps) => {
  return (
    <div style={{ marginTop: 12 }}>
      {todos.map((t) => (
        <Item
          item={t}
          key={t.id}
          deleteItem={() => {
            setTodos((prevTodos) => prevTodos.filter((td) => td.id !== t.id));
          }}
          finishItem={(isChecked: boolean) => {
            setTodos((prevTodos) => {
              const newTodos = [...prevTodos];
              const target = newTodos.find((td) => td.id === t.id);
              if (target) {
                target.finished = isChecked;
              }
              return newTodos;
            });
          }}
        />
      ))}
    </div>
  );
};
```

## Design of TodoInput

This component has 2 functions `input task name` and `add task`

```tsx
// ToDoInput.tsx
import { ToDoItem } from "../typings";

interface IToDoInputProps {
    value: string;
    onValueChange: (text: string) => void;
    addTodo: (newTodo: ToDoItem) => void;
}

export default ({value, onValueChange, addTodo}: IToDoInputProps) => {
  return (
    <div className="task-input">
      <input
        value={value}
        onChange={(e) => {
          onValueChange(e.target.value);
        }}
        placeholder="Add new task"
        className="styled-input"
      />
      <button
        className="button-3"
        onClick={() => {
          if (value) {
            const newTodo = {
              name: value,
              id: new Date().toISOString(),
              finished: false,
            };
            addTodo(newTodo);
          }
        }}
      >
        Add Task
      </button>
    </div>
  );
};
```

So, the main body of the application would be like:

```tsx
<div>
		<ToDoInput
        onValueChange={(value) => setText(value)}
        value={text}
        addTodo={(newTodo) => {
          setTodos([...todos, newTodo]);
          setText("");
        }}
      />
     <ListView todos={todos} setTodos={setTodos} />
</div>
```

But what is `setText` and  `setTodos` ?

Answer: These are the states we should manage in this application. We can initialize these 2 states like this:

```tsx
const [text, setText] = useState("");
const [todos, setTodos] = useState<ToDoItem[]>([]);

// note: ToDoItem is just a type, we will explain this later
```

```tsx
// app.tsx
import "./style.css";
import { useState } from "react";
import { ToDoItem } from "./typings";
import ToDoInput from "./components/ToDoInput";
import ListView from "./components/ListView";

export default () => {
  const [todos, setTodos] = useState<ToDoItem[]>([]);
  const [text, setText] = useState("");

  return (
    <div className="container">
      <ToDoInput
        onValueChange={(value) => setText(value)}
        value={text}
        addTodo={(newTodo) => {
          setTodos([...todos, newTodo]);
          setText("");
        }}
      />
      <ListView todos={todos} setTodos={setTodos} />
    </div>
  );
};

```
