## Problem
How to implement Pop to Root View in iOS 15 with Navigation View in SwiftUI?

## How you fix it
There are many ways to implement Pop To Root view in iOS 15 NavigationView. Simplest way which i recently used in one of my project is Define Obserable property and assign it to view on which user want to Pop to Root View or some other view. In my case I have to pop views to login and dashboard.
## Solution
```
1. Define Observable Object which is LoginState in our example.

class LoginState: ObservableObject {
    static let shared = LoginState()
    @Published var moveToRootView = false
}

2. Create @ObservedObject Property where user want to Pop to Root View.
    
    @ObservedObject var loginState = LoginState.shared

3. Set ObservedObject Property to desired view which is in our case Login()
    
    Login().id(loginState.moveToRootView)
    
4. Set and reset the @ObservedObject property from where user wants to Pop to root view.

                Text("").onAppear {
                    LoginState.shared.moveToRootView = true
                }.onDisappear() {
                    LoginState.shared.moveToRootView = false
                }

```
