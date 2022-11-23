## Problem
How to use SwiftUI View in UIKIT based Application?

## How you fix it
Using a HostingViewController, a SwiftUI view can be treated either as an entire scene (occupying the full screen) or as an individual component within an existing UIKit scene.
## Solution
```
class ViewController: UIViewController {
    
    // 2. Create a UIHostingController
    let swiftUIController = UIHostingController(rootView: SwiftUIView())

    // 1. Create the IBAction outlet
    @IBAction func goToSwiftUIView(_ sender: Any) {
        // add action
            navigationController?.pushViewController(swiftUIController, animated: true)
    }

    override func viewDidLoad() {
        super.viewDidLoad()
    }
}
```
