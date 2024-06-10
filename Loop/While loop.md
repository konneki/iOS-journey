# While loop

While loop will go on until the condition is false

```swift
var countdown = 10

while countdown < 0 {
    print("\(countdown)...")
    countdown -= 1
}

print("Boom!")
```

While loops are very useful when you don't know how many iterations will go around

```swift
var roll = 0

while roll != 20 {
    roll = Int.random(in: 1...20)
    print("I rolled \(roll)")
}

print("Rolled \(roll) - critical hit!")
```
