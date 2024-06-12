# Switch

Switch statement is useful if you have a lot of options and the `if` statement becomes unreadable.

## Example

Let's say that you have an enum with weekdays. You'd like to check which one was chosen by user and act accordingly.

```swift
enum Weekday {
    case monday, tuesday, wednesday, thursday, friday
}

var dayOfWeek: Weekday = .monday

switch dayOfWeek {
    case .monday:
        print("Hate this day tbh") // Hate this day tbh
    case .tuesday:
        print("Better than yesterday")
    case .wednesday:
        print("Time for ice cream")
    case .thursday:
        print("Little friday")
    case .friday:
        print("Time to start weekend")
}
```

### Important

Remember that switch statement is **explicit**, which means that all options should be taken into consideration. Swift won't complain for the above code, because every single option of an enum was taken into consideration.

Let's say that we check a number.

```swift
let number = 10

switch number {
    case self > 5:
        print("Big boiiiii") // Big boiiiii
    default:
        print("Small boiiiii")
}
```

The `default` option is mandatory in such options to avoid broken code.
