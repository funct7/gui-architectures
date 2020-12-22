# Redux

## [Terms and Concepts](https://redux.js.org/tutorials/essentials/part-1-overview-concepts#redux-terms-and-concepts)

### Core Principles

- **Single source of truth**: The state of your whole application is stored in an object tree within a single store.
- **State is read-only**: The only way to change the state is to emit an action, an object describing what happened.
- **Changes are made with pure functions**: To specify how the state tree is transformed by actions, you write pure reducers.

### Components

- **State**: The source of truth that drives an app.
- **View**: A declarative description of the UI based on the current state.
- **Action**: The events that occur in the app based on user input, and trigger updates in the state.

### **Actions**

An action is an event that describes something that happened in the application.
- Form
  ```ts
  interface Action {
     type: string; // Format: 'domain/eventName'
     payload: any; // Additional information about what happened.
  }
  ```
- Action creators: Factory methods for actions
  ```js
  const addTodo = text => { 
     return { type: 'todos/todoAdded', payload: text };
  }
  ```
  
### **Reducers**
A reducer is a function that receives the current state and an action object, decides how to update the state if necessary, and returns the new state.

#### Contraints
- Reducers **should** only calculate the new state value based on the state and action arguments.
- Reducers are **not** allowed to modify the existing state. Instead, they must make immutable updates, by copying the existing state and making changes to the copied values.
- Reducers **must not** do any asynchronous logic, calculate random values, or cause other "side effects"

#### [Rationale](https://redux.js.org/tutorials/essentials/part-2-app-structure#rules-of-reducers)
> But why are these rules important? There's a few different reasons:
>
> - One of the goals of Redux is to make your code predictable. When a function's output is only calculated from the input arguments, it's easier to understand how that code works, and to test it.
> - On the other hand, *if a function depends on variables outside itself, or behaves randomly, you never know what will happen when you run it.*
> - *If a function modifies other values, including its arguments, that can change the way the application works unexpectedly.* This can be a common source of bugs, such as "I updated my state, but now my UI isn't updating when it should!"
>
> ... (Emphasis added.)

#### Thunk
Async logic is typically written in special functions called "thunks". A thunk is a specific kind of Redux function that can contain asynchronous logic.

Thunks are written using two functions:
- An inside thunk function, which gets dispatch and getState as arguments
- The outside creator function, which creates and returns the thunk function

```js
// the outside "thunk creator" function
const fetchUserById = userId => {
  // the inside "thunk function"
  return async (dispatch, getState) => {
    try {
      // make an async call in the thunk
      const user = await userAPI.fetchById(userId);
      // dispatch an action when we get the response back
      dispatch(userLoaded(user));
    } catch (err) {
      // If something went wrong, handle it here
    }
  };
};
```


### **Store**
The current Redux application state lives in an object called the store .

- The store is created by passing in a reducer
- The store has a method called `getState` that returns the current state value.
- The Redux store has a method called `dispatch`. The only way to update the state is to call `store.dispatch()` and pass in an action object.

### **Selectors**

Selectors are functions that know how to extract specific pieces of information from a store state value.


# Reflection

1. > One way to solve this is to extract the shared state from the components, and put it into a centralized location outside the component tree. With this, our component tree becomes a big "view", and **any component can access the state or trigger actions, no matter where they are in the tree!** (Emphasis added.)

   The limitation imposed by "any component accessing the state or triggering actions" is that the component is not made for generic purposes but is specific to that application. If components were to be used across applications, they must be state/action agnostic, and the best way to achieve this is to function as mere event handlers.

2. Redux sounds like an event loop where `action` is messages sent from threads. `Reducer`s are listeners repsonding to message dispatches.

3. > - **Any React component can use data from the Redux store as needed**
   >   - Any component can read any data that is in the Redux store
   >   - Multiple components can read the same data, even at the same time
   >   - Components should extract the smallest amount of data they need to render themselves
   >   - Components can combine values from props, state, and the Redux store to determine what UI they need to render. They can read multiple pieces of data from the store, and reshape the data as needed for display.
   >   - Any component can dispatch actions to cause state updates

   By allowing this, all components are subject to changes to the store; i.e. there is no layer between the store data and the presentation logic. Also, by allowing all and any component to dispatch actions, it gets hard to track which where a change originated.

# Question

- Unnecessary persistence, and manual reset of session state.

  - Data used by a subset of components are persisted throughout the lifetime of the application.
  - Bugs arise when a user signs out, and another user signs in, while the global state is not reset.

- Components directly accessing the global state makes things hard to refactor.

  - Case: A tabular component was changed to support sort on fields.
    - The children components directly accessed data on the store.
    - There is no guarantee that an in-place sort will affect other components.
    - The feature was implemented by the parent component making a copy of the list, sorting it, and providing the list as props on the children.
    - Most components worked fine, but there was a bug. A component a few more steps down accessed the list on the store directly.
    
  - While it could be argued that tests would have prevented it, would all tests guarantee the right behavior? Tests would have to check each field that it was sorted with the rest of the fields.
