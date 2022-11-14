## Problem
How to use share Data with all App Views without using Singleton object.

## How you fix it
SwiftUI gives us the @EnvironmentObject property wrapper for data that should be shared with many views in your app. This lets us share model data anywhere it’s needed, while also ensuring that our views automatically stay updated when that data changes.
## Solution
```
Step 1: Create observable object class

class AppSettings: ObservableObject {
    @Published var userId = 0

Step 2: Creates the AppSettings object, set value and places it into the environment for the navigation view.
struct ContentView: View {
    @StateObject var settings = AppSettings()

    var body: some View {
        NavigationView {
            VStack {
                // A button that writes to the environment settings
                Button("Login") {
                    settings.userId = viewModel.user.userId
                }

                NavigationLink(destination: DashboardView()) {
                    Text("Show Dashboard View")
                }
            }
            .frame(height: 200)
        }
        .environmentObject(settings)
    }
}

Step 3: Use AppSettings object within a view that expects to find a userId in the environment.
struct DashboardView: View {
    @EnvironmentObject var settings: AppSettings

    var body: some View {
        Text("UserId: \(settings.userId")
    }
}

Tips: If you are using EnvironmentObject in OnAppear method then use asyncAfter(deadline: .now() + 1.0)
Also if navigationView not worked you can pass settings object in DashboardView().environmentObject(settings)

```
