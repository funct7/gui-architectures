# Flux

### Components

- Consists of Action, Dispatcher, Store, and View.
- Dispatcher, stores, and views are independent nodes with distinct inputs and outputs.

- Action
  - Simple objects containing new data and an identifying type property.
  - Provided to the dispatcher in an "action creator" method.
- Dispatcher
  - A central hub
  - On receiving actions, the dispatcher invokes the callbacks that the stores have registered with it.
- Store
  - Within callbacks, stores respond to relevant actions and change state.
  - Stores then emit a "change event" to alert the controller-views.
- View
  - Controller-views observe data change events and retrieve data from the stores in an event handler.
  - With the new data, views update their view hierarchy.

# Questions

1. How are actions different from calling methods?
2. How do two-way data bidnings lead to cascading updates?
