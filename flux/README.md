# Flux

### Components

- Consists of **Action**, **Dispatcher**, **Store**, and **View**.
- Dispatcher, stores, and views are independent nodes with distinct inputs and outputs.

#### Action

- **Simple objects containing new data and an identifying type property**.
- Provided to the dispatcher in an "action creator" method.
- Actions may be invoked from within views' event handlers, so that they are called in response to a user interaction.

#### Dispatcher

- **A central hub that simply distributes actions to stores**.
- Each store registers itself and provides a callback.
- On receiving actions, the dispatcher invokes the callbacks that the stores have registered with it.
- It's the reponsibility of the dispatcher to manage dependencies between stores by invoking the registered callbacks in a specific order.

#### Store

- **Stores contain the application state and logic for a particular domain within the application.**
  - Q: Are they similar to `Repositories` in Google MVVM architecture?
- Within callbacks, stores `switch` on action types, respond to relevant actions and change state.
- Stores then emit a "change event" to alert the controller-views.

#### View and Controller-Views

- Controller-views are special kind of observer views at the top of a nested view hierarchy.
- Controller views govern any significant section of a page.
- Controller-views observe data change events and retrieve data from the stores.
- Data is requrested from the stores through the stores' public getter methods.
- With the new data, views update their view hierarchy.

- Controller-views may exist within the view hierarchy, but this should be done with caustion. Having conflicting entry points for the data flow may cause difficulty of debugging.

# Questions

1. How are actions different from calling methods?
2. How do two-way data bindings lead to cascading updates?
3. If a dispatcher knows how stores depend on each other, does that mean there is logic in dispatchers as well as stores?

   - That is, how is the dispatcher supposed to know which stores depend on each other when it's the store's callback implementation that decides which actions they respond to?

     From [Flux page on dispatchers](https://facebook.github.io/flux/docs/in-depth-overview/#a-single-dispatcher):

     > It is essentially a registry of callbacks into the stores and has no real intelligence of its own — it is a simple mechanism for distributing the actions to the stores.

     Also in [Flux page on stores](https://facebook.github.io/flux/docs/in-depth-overview/#stores):

     > Within the store's registered callback, a switch statement based on the action's type is used to interpret the action and to provide the proper hooks into the store's internal methods.

     The internals of the callbacks are not exposed to the dispatchers, but dispatchers are used to control the specific sequence of store callbacks to call on. This seems like tight coupling as well as knowing the implementation of an independent component.

     **The dispatcher says it knows nothing about whether an action is consumed by the store but knows about the dependencies between stores.** How is this possible?

4. A question that must be asked to all self-data-binding views: How do the views know which event they should observe? Also, these views are tightly coupled to a specific store, so they cannot be reused in other screens where the subsection of a screen has the same UI. (Logic duplication).

5. Related to `4`, if stores do not provide raw model but view models (models adapted to the needs of views), this means there is presentation logic in stores as well as views. (Responsibility separation)

6. Along with `4`, can the programmer know which parts of the screen are affected by a view state update event? In order to check which views are updated for an update of a view state, a text search should be done across files. (Logic cohesiveness).

7. Who sets the event handlers on views? Those event handlers know which user interaction must call which action.

# Reference

- [Flux page](https://facebook.github.io/flux/docs/in-depth-overview/)
