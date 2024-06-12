# `break` and `continue` in loops

## Differences

`continue` skips the current iteration and continues with the rest of them. `break` escapes the loop at the current iteration.

```swift
let filenames = ["flower.jpg". "text.txt", "car.jpg"]

for filename in filenames {
    if filename.hasSuffix("jpg") == false {
        continue
    }

    print("Picture file found: \(filename)")
}

let number1 = 4
let number2 = 14
var multiples = [Int]()

for i in 1...100_000 {
    if i.isMultiple(of: number1) && i.isMultiple(of: number2) {
        multiples.append(i)

        if multiples.count == 10 {
            break
        }
    }
}

print(multiples)
```
