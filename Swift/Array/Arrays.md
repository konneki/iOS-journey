# Arrays

Arrays are complex data types that store ordered list of items. When initiating a variable or a constant with existing data, Swift automatically assign the type which is called _type inference_.

```swift
var stringArray = ["item 1", "item 2"]
var intArray = [1, 2, 3]

// to make intArray a doubleArray it should use type annotation
var doubleArray: [Double] = [1, 2, 3]
```

It is important to know that an array may only contain **the same type** inside. Code like below would throw an error:

```swift
var array = ["item 1", 2, true]
```

It is possible to assign the `Any` type, but it is better not to use it. Swift is checking for the type that is assigned to the array, which works like a guard. It throws an error when you try to add a value of different type.

## Adding/removing array items

To add a value to the end of the array you can use `append`. You may also insert items at specific index. You can remove items from specific index using `remove`.

```swift
var animals = ["cats", "dogs", "chimps", "moose"]
animals.append("geese") // ["cats", "dogs", "chimps", "moose", "geese"]
animals.insert("fish", at: 1) // ["cats", "fish", "dogs", "chimps", "moose", "geese"]
animals.remove(at: 2) // ["cats", "fish", "chimps", "moose", "geese"]
```

It is very important to use indexes you are sure exists. Swift will complain loudly when trying to do anything with array items that does not exist. You may always check if that's true by using `indices.contains(x)`.

```swift
if array.indices.contains(4) {
    // do something
} else {
    // do something else
}
```

## Counting items in the array

You may use `count` to check the number of items in the array. When using it in the large array Swift needs to go through each individual item every time which is time and resource consuming.

```swift
var array = [1, 2, 3, 4, 5, 6, 7]
print(array.count) // 7
```
