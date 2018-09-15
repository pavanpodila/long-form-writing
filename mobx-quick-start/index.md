# MobX Quick Start Guide

## Introduction to the world of MobX

**State Management** is an aspect of your application that controls what is ultimately rendered. It's the pumping heart that spreads data across your app and brings various components to life. The _state_ of a Web Application, like the ones built with `React.js`, rely on the central Store as the _single source of truth_. Actions originating in the UI are fired on this store to modify the state. This results in a new version of the state, which is notified back to the UI by the Store. This cycle of _Actions --> Store --> UI_ is always uni-directional and makes it easy to understand the data-flow in the application.

MobX embraces this idea and provides a model of Transparent Functional Reactive Programming (TFRP) that eliminates much of the boilerplate in setting up this data-flow. MobX is a state-management framework that comes with a simple, yet powerful set of building blocks, such as Observables, Actions and Reactions. These three concepts acts as the junctions of the data-flow triad.

![Observables-Actions-Reactions triad](mobx-triad.png)

Observables carry the central mutable-state of your application. As this state changes, it fires notifications that can be observed by other participants (aka observers) of the data-flow triad. Reactions are the primary observers in the MobX framework. The UI (React Components) is the most glorious observer in the MobX system. These components observe the observable-state and render the visual representations of data.

> Reacting to a change in observable-state is also known as a **_side-effect_**. The UI is the most visible _side-effect_ in your application.

With `observables` carrying the reactive, mutable state of your application and `reactions` acting as the side-effects, we are missing the third piece: **actions**. Actions are the mutating functions that cause the real changes to the observable state. A React Component cannot (and should not) modify the mutable state directly. Instead, it fires `actions`, which internally cause the desired changes to the observables.

## State Management is hard

As your application scales in size, it becomes necessary to pay close attention to how state is being changed from different parts of your application. It is quite common to have several async operations going on in your app, such as network calls, timers or promise-based operations. Managing the success, failure states of these operations and ensuring that the UI is not going out of sync, can become challenging. With MobX, you can map these to a set of observables, actions and reactions and wire them together without any boilerplate.

Consider this snippet of MobX that wires a `TodoComponent` (the UI, aka reaction) to a `TodoStore` (the observable). By firing _actions_ from the UI, we mutate the state of the Todo, which gets observed by `TodoComponent` to re-render the state.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import { observable, action } from 'mobx';
import { observer } from 'mobx-react';

class Todo {
    @observable
    done = false;

    @observabble
    title = '';

    @action
    toggleDone = () => {
        this.done = !this.done;
    };

    @action
    setTitle(title) {
        this.title = title;
    }
}

@observer
class TodoComponent {
    render() {
        <div
            style={{
                display: 'flex',
                flexDirection: 'row',
                alignItems: 'center',
            }}
        >
            <input
                type="checkbox"
                checked={store.done}
                onChange={store.toggleDone}
            />
            <input
                type="text"
                style={{ flex: 1 }}
                value={store.title}
                onChange={event => store.setTitle(event.target.value)}
            />
        </div>;
    }
}

const store = new Todo();
class App extends React.Component {
    render() {
        return <Todo store={store} />;
    }
}

ReactDOM.render(<App />, document.getElementById('root'));
```

Notice the use of decorators such as @observable, @action, @observer that sets up the connections between the Store, React Components and the firing of actions. This is the minimal and essential boilerplate you have to write to create a React Component that automatically re-renders itself anytime the done or title properties change.
