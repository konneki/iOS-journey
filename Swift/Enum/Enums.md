# Enums

`enum` - short of _enumeration_ - is a set of named values that you can create and use in code.
Let's say that your code needs to check days of week.
Instead of using:

```swift
if (dayOfWeek == "monday") {
    print("Oh no, it's Monday ;_;")
}
```

As this may come with user errors, that instead of using lower case `monday` they can use `Monday` or even worse with a spelling error `Mo nday` which will break the code you wrote.

This is where **enums** come in:

```swift
enum Weekday {
    case monday
    case tuesday
    case wednesday
    case thursday
    case friday
}
```

Or in short you can type in

```swift
enum Weekday {
    case monday, tuesday, wednesday, thursday, friday
}
```

Now you can use explicitly the options that you created which is error-prone.

```swift
var dayOfWeek = Weekday.monday
dayOfWeek = .tuesday

if (dayOfWeek == .monday) {
    print("Oh no, it's Monday ;_;")
}
```

In above example instead of using `Weekday.monday` everywhere Swift knows that the type annotation of `dayOfWeek` variable is `Weekday`, so checking only their values by `.monday` etc. is okay. Now user is not able to make errors.
