## Problem
How to add Reusable view components in SwiftUI?

## How you fix it
There are many ways to reuse view components in SwiftUI. With SwiftUI’s declarative way of building views, it doesn’t take much to make the view components reusable. This is a big improvement from the traditional way of managing Interface Builder files.
    Following are 5 ways to resuse components:
## Solution
```
1. Local or global reusability

@State private var isMorning = true 
private var greetingText: some View {
    Text(isMorning ? "Good morning!" : "Good afternoon!")
        .font(.appTitle)
}

2. View Extensions

extension Text {
    static func greetingText(isMorning: Bool) -> Text {
        return Text(isMorning ? "Good morning!" : "Good afternoon!")
            .font(.appTitle)
    }
}

3. Custom Styling 
struct ContentView: View {
    var body: some View {
        Button("Tap me", action: {})
            .buttonStyle(AppButtonStyle())
    }
}
 
struct AppButtonStyle: ButtonStyle {
    
    let buttonFont = Font.custom("Zilla Slab", size: 20).weight(.bold)
    
    func makeBody(configuration: Self.Configuration) -> some View {
        configuration
            .label
            .font(buttonFont)
            .multilineTextAlignment(.center)
            .lineLimit(1)
            .padding(.horizontal, 10)
            .foregroundColor(.white)
            .offset(y: -1)
            .frame(height: 30)
            .background(Color.black)
            .clipShape(RoundedRectangle(cornerRadius: 10))
            .scaleEffect(configuration.isPressed ? 0.9 : 1)
            .opacity(configuration.isPressed ? 0.6 : 1)
            .animation(.spring())
    }
}

4. ViewModifier
struct ContentView: View {
    var body: some View {
        Text("SwiftUI is awesome!").appLabel()
    }
}
 
extension Text {
    func appLabel() -> some View {
        self.modifier(AppLabel())
    }
}
 
struct AppLabel: ViewModifier {
    
    let labelFont = Font.custom("Zilla Slab", size: 14).weight(.regular)
    
    func body(content: Content) -> some View {
        content
            .padding()
            .foregroundColor(.red)
            .font(labelFont)
    }
}

5. ViewBuilder
struct ContentView: View {
    var body: some View {
        BaseViewContainer {
            Text("I'm inside a container!")
            Image(systemName: "hand.thumbsup")
        }
    }
}
 
struct BaseViewContainer <Content : View> : View {
    var content : Content
    
    init(@ViewBuilder content: () -> Content) {
        self.content = content()
    }
    
    let labelFont = Font.custom("Zilla Slab", size: 14).weight(.regular)
    var body: some View {
        content
            .padding()
            .foregroundColor(.red)
            .font(labelFont)
            .overlay(
                RoundedRectangle(cornerRadius: 10)
                    .stroke(Color.red, lineWidth: 2)
            )
    }
}

```
