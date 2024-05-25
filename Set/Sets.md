# Sets

Sets are very similar to Arrays, with the difference, that they do not store data with particular order and also they store unique values (cannot store duplicates).

## Creating sets

Set creation is almost identical to Arrays.

```swift
let people = Set(["Denzel Washington", "Tom Cruise", "Nicolas Cage", "Samuel L Jackson"])
```

When you want to print that set, you may encounter result with different order.

```swift
print(people) // ["Nicolas Cage", "Tom Cruise", "Samuel L Jackson", "Denzel Washington"]
print(people) // ["Samuel L Jackson", "Nicolas Cage", "Denzel Washington", "Tom Cruise"]
```
