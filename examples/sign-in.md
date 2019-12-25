# The Sign-in Example

## Requirement

- A screen with username and password input fields.
- A sign-in button that is only enabled when both input fields are valid.

## MVVM

### On User Input

1. `V`(input fields) call commands on the `VM`.
2. `VM` updates `M` with the input values provided by `V`.
3. `M` validates input and notifies `VM` if data changed.
4. `VM` publishes the update event.
5. `V`s subscribing to the update event update their views accordingly.
