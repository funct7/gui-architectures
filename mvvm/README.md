# MVVM

-   Reference: [Microsoft MVVM GitHub repo](https://github.com/microsoft/InventorySample/tree/master/docs/MVVM)

### Questions

- How dumb should the V be?
  - Case 1: A screen where the user can choose "TODAY" view or "MONTH" view. The selector view binds to the `currentDateRange: String` value. Should there be a separate boolean flag for `todayView.isHidden`?
  - Case 1-2: The user changes the date range from "TODAY" view to "MONTH" view. What should be sent to the `VM`? How should the updated `VM` state for the `currentDateRange` be bound in the `V`? A segmented selector enumerates all texts it has and the one that matches the new value is selected?

  - Case 2: An item shows the name, the picture, and the status of a user. If the user is the currently signed-in user, the text appears in bold and there is ` (me)` appended to the name.
    - The name can have a value of `item.name = "\(user.name) (me)"`, but how is the bold-ness applied?
    - If the item has a flag `isMe: Bool`, the `V` is taking part in the presentation logic, which should be the role of a `VM`. Where is the line?

  - Case 3: There is a calendar that allows for scrolling up to two pages either side. Once the calendar scrolling stops, whatever month the scroll is focused on becomes the middle value; i.e. when scrolling stops, the months that are available is `[-2, -1, 0, 1, 2]` of the focused month. The scroll offset is reset to the middle, so a pseudo-infinite scroll behavior is implemented.

    - How is the view supposed to show the other calendars? >> The `VM` can take on the role of the data source.
    - There is an independent header showing the month of the focused month of the calendar; e.g. the header shows `Nov 2020`. Should the `VM` provide the list of month names for the `V`?
    - Given the calendar, should the `VM` provide which month the focus should go to; i.e. the scroll offset?

### Possible Variants

- Dumb View Model
  - This variant sees the `VM` as a dumb data holder that the `V` can bind to.
  - The `VM` has no methods. Any mutations are done as a whole, and it's done by a "higher being". This higher being could be another `VM` or some function, etc.
  - Because mutations are done as a whole, the properties that the `VM` holds are not observed separately, but the whole `VM` is observed.
  - A widely adopted version of this is Redux.

  - A possible advantage of this is that the intermediate state is never exposed since the `VM` change is notified as a whole.
  - A possible disadvantage of this variant is that views need additional "diffing" in order to make view updates smooth. 

- Classic View Model
  - This is the original `VM` where the `VM` has commands the `V` can invoke in order to request data changes.
  - Since the `VM` handles data change requests--ideally through requesting data change to the `M` and observing its changes on properties,--it may be natural for the `VM` to be self-mutating, thus having its properties observed separately.
    - The `VM` might replace itself as a whole, making it a non-mutable `VM` with commands.

  - A common pitfall in implementing this `VM` is notifying property changes when it is still in an intermediate/transitioning state; i.e. when two properties must change at the same time, a property change might be notified, exposing an illegal state to the `V`.
