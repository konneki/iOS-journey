# Protocols

Protocols are a bit like contracts in Swift: they let us define what kinds of functionality we expect a data type to support, and Swift ensures that the rest of our code follows those rules.

```swift
protocol Vehicle {
    func estimateTime(for distance: Int) -> Int
    func travel(distance: Int)
}
```

Protocols let us define a series of properties and methods that we want to use. They don’t implement those properties and methods – they don’t actually put any code behind it – they just say that the properties and methods must exist, a bit like a blueprint.

1. To create a new protocol we write protocol followed by the protocol name. This is a new type, so we need to use camel case starting with an uppercase letter.
2. Inside the protocol we list all the methods we require in order for this protocol to work the way we expect.
3. These methods do not contain any code inside – there are no function bodies provided here. Instead, we’re specifying the method names, parameters, and return types. You can also mark methods as being throwing or mutating if needed.

We can design types that work with that protocol. This means creating new structs, classes, or enums that implement the requirements for that protocol, which is a process we call _adopting_ or _conforming_ to the protocol.

```swift
struct Car: Vehicle {
    func estimateTime(for distance: Int) -> Int {
        distance / 50
    }

    func travel(distance: Int) {
        print("I'm driving \(distance)km.")
    }

    func openSunroof() {
        print("It's a nice day!")
    }
}
```

Draw particular attention to in that code:

1. We tell Swift that **Car** conforms to **Vehicle** by using a colon after the name **Car**, just like how we mark subclasses.
2. All the methods we listed in **Vehicle** must exist _exactly_ in **Car**. If they have slightly different names, accept different parameters, have different return types, etc, then Swift will say we haven’t conformed to the protocol.
3. The methods in **Car** provide actual implementations of the methods we defined in the protocol. In this case, that means our struct provides a rough estimate for how many minutes it takes to drive a certain distance, and prints a message when **travel()** is called.
4. The **openSunroof()** method doesn’t come from the **Vehicle** protocol, and doesn’t really make sense there because many vehicle types don’t have a sunroof. But that’s okay, because the protocol describes only the minimum functionality conforming types must have, and they can add their own as needed.

We could use a code like this:

```swift
func commute(distance: Int, using vehicle: Car) {
    if vehicle.estimateTime(for: distance) > 100 {
        print("That's too slow! I'll try a different vehicle.")
    } else {
        vehicle.travel(distance: distance)
    }
}

let car = Car()
commute(distance: 100, using: car) // I'm driving 100km.
```

But that code would work exactly the same if we did not include protocols, so why should we use them?

Here comes the clever part: Swift knows that any type conforming to **Vehicle** must implement both the **estimateTime()** and **travel()** methods, and so it actually lets us use **Vehicle** as the type of our parameter rather than **Car**. We can rewrite the function to this:

```swift
func commute(distance: Int, using vehicle: Vehicle) {
    if vehicle.estimateTime(for: distance) > 100 {
        print("That's too slow! I'll try a different vehicle.")
    } else {
        vehicle.travel(distance: distance)
    }
}
```

Now the function would work on any vehicle we provide unless it conforms to **Vehicle** protocol.

```swift
struct Bicycle: Vehicle {
    func estimateTime(for distance: Int) -> Int {
        distance / 10
    }

    func travel(distance: Int) {
        print("I'm cycling \(distance)km.")
    }
}

let bike = Bicycle()
commute(distance: 50, using: bike) // I'm cycling 50km.
```

Now we have a second struct that also conforms to **Vehicle**, and this is where the power of protocols becomes apparent: we can now either pass a **Car** or a **Bicycle** into the **commute()** function. Internally the function can have all the logic it wants, and when it calls **estimateTime()** or **travel()** Swift will automatically use the appropriate one – if we pass in a car it will say “I’m driving”, but if we pass in a bike it will say “I’m cycling”.

As well as methods, you can also write protocols to describe properties that must exist on conforming types. To do this, write **var**, then a property name, then list whether it should be readable and/or writeable.

```swift
protocol Vehicle {
    var name: String { get }
    var currentPassengers: Int { get set }
    func estimateTime(for distance: Int) -> Int
    func travel(distance: Int)
}
```

That adds two properties:

1. A string called **name**, which must be readable. That might mean it’s a constant, but it might also be a computed property with a getter.
2. An integer called **currentPassengers**, which must be read-write. That might mean it’s a variable, but it might also be a computed property with a getter and setter.

So now our protocol requires two methods and two properties, meaning that all conforming types must implement those four things in order for our code to work. This in turn means Swift knows for sure that functionality is present, so we can write code relying on it.

For example, we could write a method that accepts an array of vehicles and uses it to calculate estimates across a range of options:

```swift
func getTravelEstimates(using vehicles: [Vehicle], distance: Int) {
    for vehicle in vehicles {
        let estimate = vehicle.estimateTime(for: distance)
        print("\(vehicle.name): \(estimate) hours to travel \(distance)km")
    }
}

getTravelEstimates(using: [car, bike], distance: 150)
// Car: 3 hours to travel 150km
// Bike: 15 hours to travel 150km
```

You can conform to as many protocols as you need, just by listing them one by one separated with a comma. If you ever need to subclass something _and_ conform to a protocol, you should put the parent class name first, then write your protocols afterwards.
