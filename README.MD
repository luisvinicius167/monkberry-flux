# monkberry-flux <br/>
[![npm package](https://img.shields.io/badge/npm-0.0.1-blue.svg)](https://www.npmjs.com/package/monkberry-flux)
> A Reactive library for Monkberry applications or any Javascript application, inspired by Flux.

### Why monkberry-flux?
**monkberry-flux** is a reactive library that provides for your application multiple stores, wich each store manage your state.

<pre align="center">
╔═════════╗       ╔════════════╗       ╔═══════════╗       ╔═════════════════╗
║  Store  ║──────>║  Dispatch  ║ ────> ║   State   ║ ────> ║ View Components ║
╚═════════╝       ╚════════════╝       ╚═══════════╝       ╚═════════════════╝

</pre>

### Install
* Npm: ``` npm install monkberry-flux ```

### Reasons for use
* Tiny size: ~1kb
* Best Performance
* Reactive Stores
* Simple and minimalistic API
* Unidirectional data flow

### Data Flow
In **monkberry-flux** data flow is unidirectional, as it should be in Flux:

* The store dispatch her actions
* Actions change the state.
* When state changes you can trigger a handler

### Principles:
* Application state is held in the store, as a single object.
* The only way to mutate the state is by dispatching store actions.
* Actions must be synchronous, and the only side effects they produce should be mutating the state.

### Stores:
A **Store** is basically a container that holds your application state. There are two things that makes a **monkberry-flux** store different:

 * A Store is **reactive**. Every time the state changes, you can trigger a handler.

 * You cannot directly change the store's **state**. The only way to change a store's state is by explicitly dispatching store actions.

Creating a Store is pretty straightforward - just provide an name, state and actions:

```javascript
import monkberry-flux from "monkberry-flux";

const TodoStore = monkberryFlux.createStore({
  name: 'Todo',
  state:  {
    todos: []
  },
  actions: {
    ADD_TASK ( state, task ) {
      state.todos.push(task);
    },
    REMOVE_TASK ( state, index ) {
      state.todos.splice(index, 1);
    }
  }
});

// every time the state changes, this function will be triggered 
TodoStore.observe( templates => {
  // update each template that are subscribed on the store with
  // the new store state
  templates.forEach( template => template.update(TodoStore.getState());
})

module.exports = TodoStore;
```

#### State
Application state is held in the store, as a single object. **monkberry-flux** uses a **single state tree** - that is, this single object contains all your Store level state and serves as the *"single source of truth"*.

### Store Actions
Actions are just functions that call the store actions. **All actions receive a state as first argument**.

Creating an action inside your Store:

```javascript
actions: {
  increment ( state, n ){
    state.count += n
}
```
#### Calling an action on your component

```javascript
import { TodoStore } from './store/todo';

TodoStore.dispatch('todos', 'ADD_TASK', 10);
```

An ```action``` receives the **state** property that you want to change as first argument, the ***action event name** as the second argument, anything after these are passed as arguments to the action callback.

### Get
To get the store state value, use ```store.getState()``` or ```store.getState( name )```, in your Components or Stores.

### Application Structure
Just suggesting.

```project
├──index.html
├──templates
|   ├──component.monk
|   ├──component.js
├──stores
|   ├──todo.js
|   ├──api.js
```

### API Reference

* #### Create a Store:
  * ``` monkberryFlux.createStore({ name, state, actions }) ```: Create a single store with the name of Store, State and Actions.

* #### Store Actions:
  * ``` store.dispatch(stateName, action [,...arguments]) ```: Call a store action.

* #### Add you store handler to be called when the state changes:
  * ``` store.observe(listeners, stateName, stateValue) ```: Register a handler that will be triggered when any state change in you store.


* #### Observing the store state changes in your Component or other Store:
  * ``` yourStore.subscribe( listener ) ```: Add the listener for watch the Store state changes.
  * ``` yourStore.unsubscribe( listener ) ```: Remove the listener for unwatch the Store state changes.
  * ``` yourStore.getState(state?) ```: Gets a value of the store state that you passed as argument or all state if argument are present.


### License
MIT License.
