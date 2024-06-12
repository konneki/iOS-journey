# Dictionaries

Dictionaries are complex data types, which can store data of different type as a key-value pair. Unlike the Arrays, the values can be received by providing the key.

```swift
let myDict = ["name": "Daniel Konnek", "age": "27", "isSingle": "false"]
```

## Work with dictionary values

Even though you can specify the keys of the dictionary it does not throw an error when you reach for the value of it when the key does not exist. This is because a dictionary may or may not contain specific key for that point in time. This also means that when a key exists, the value becomes optional.

```swift
print(myDict["surname"]) // nil
print(myDict["name"]) // Optional("Daniel Konnek")
```

The value is an optional, because it may or may not contain a value. To resolve this, you can add a default value.

```swift
print(myDict["name", default: "Unknown"]) // Daniel Konnek
print(myDict["surname", default: "Unknown"]) // Unknown

if let name = myDict["name"] {
    print(name) // Daniel Konnek
}
```
