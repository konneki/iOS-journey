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

To create an empty set to fill out later you use below syntax.

```swift
var people = Set<String>()
people.insert("Denzel Washington")
people.insert("Tom Cruise")
people.insert("Nicolas Cage")
people.insert("Samuel L Jackson")
```

Instead of using `append` the `insert` is used, as you cannot append a value as the order is never created. The result is exactly the same as at the beginning of the document.

## Why use Sets when Arrays exists

Even though Sets work very similar to Arrays, there is one key difference that may be crucial. It's the speed of use. Imagine that you have an Array with 1000000 items inside. You'd like to find one or just count them. To count them Swift need to count each individual index. Sets are different. Because they do not store data with order it is very quick to count them and also find an item within Set.
