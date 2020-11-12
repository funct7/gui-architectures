# MVVM

-   Reference: [Microsoft MVVM GitHub repo](https://github.com/microsoft/InventorySample/tree/master/docs/MVVM)

### Questions

- How dumb should the V be?
  - Case 1: A screen where the user can choose "TODAY" view or "MONTH" view. The selector view binds to the `currentDateRange: String` value. Should there be a separate boolean flag for `todayView.isHidden`?
  - Case 2: An item shows the name, the picture, and the status of a user. If the user is the currently signed-in user, the text appears in bold and there is ` (me)` appended to the name.
    - The name can have a value of `item.name = "\(user.name) (me)"`, but how is the bold-ness applied?
    - If the item has a flag `isMe: Bool`, the `V` is taking part in the presentation logic, which should be the role of a `VM`. Where is the line?