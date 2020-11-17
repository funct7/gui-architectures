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