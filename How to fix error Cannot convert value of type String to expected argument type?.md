## Problem
How to fix “Cannot convert value of type 'String' to expected argument type Binding<String>”?


## How you fix it
SwiftUI’s components expect to have two-way bindings to properties, using property attributes like @State or @ObservedObject we can bind object two way. This error occurs because you tried to create an interactive component without a binding, such as this:
TextField("Username", text: name)

## Solution
```
To fix the problem, make the property attribute @State or @ObservedObject, like this:
@State private var name = ""
TextField("Usernmae", text: $name)

```
