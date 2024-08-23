# Opaque return type

Swift provides one really obscure, really complex, but really important feature called opaque return types, which let us remove complexity in our code. Honestly I wouldn’t cover it in a beginners course if it weren’t for one very important fact: you will see it immediately as soon as you create your very first SwiftUI project.

**Important:** You don’t need to understand in detail how opaque return types work, only that they exist and do a very specific job. As you’re following along here you might start to wonder why this feature is useful, but trust me: it is important, and it is useful, so try to power through!

Returning an opaque return type means we still get to focus on the functionality we want to return rather than the specific type, which in turn allows us to change our mind in the future without breaking the rest of our code.

```swift
func getRandomNumber() -> some Equatable {
    Int.random(in: 1...6)
}

func getRandomBool() -> some Equatable {
    Bool.random()
}
```

Function **getRandomNumber()** could switch to using Double.random(in:) and the code would still work great.

The advantage here is that Swift always knows the real underlying data type.

We might say something like this: _there’s a screen with a toolbar at the top, a tab bar at the bottom, and in the middle is a scrolling grid of color icons, each of which has a label below saying what the icon means written in a bold font, and when you tap an icon a message appears._

When SwiftUI asks for our layout, that description – the whole thing – becomes the return type for the layout. We need to be explicit about every single thing we want to show on the screen, including positions, colors, font sizes, and more. Can you imagine typing that as your return type? It would be a mile long! And every time you changed the code to generate your layout, you’d need to change the return type to match.

This is where opaque return types come to the rescue: we can return the type **some View**, which means that some kind of view screen will be returned but we don’t want to have to write out its mile-long type. At the same time, Swift knows the real underlying type because that’s how opaque return types work: Swift always knows the exact type of data being sent back, and SwiftUI will use that to create its layouts.
