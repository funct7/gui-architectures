# The Sign-in Example

- Username/Password input fields
- A button that is only enabled when both input fields are valid

## MVVM

### On User Input

1. `V`(input fields) triggers commands on the `VM`.
2. `VM` updates `M`.
3. `M` validates and notifies `VM` that data changed.
4. `VM` publishes the update event.
5. `V`s subscribing to the update event update their views accordingly.
