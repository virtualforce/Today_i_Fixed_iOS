# Problem
Main ChatView (Displaying messages) is not changing the vertical size dynamically when the keyboard is displayed or hidden


# Environment
- SwiftUI
- Minimum iOS version 14.0


# How you fix it
=> I will focus only on making the chat view dynamic to handle Keyboard events and adjust the size accordingly. I am assuming you already have a chat view.  
First Make a class that conforms to **ObservableObject** in my case it is **KeyboardResponder** Now add it as an observer of keyboard notifications by iOS and get the current height.
Make an **@ObservedObject** of KeyboardResponder to use this in **.padding** & **.edgesIgnoringSafeArea** modifiers of ChatView

I have provided the code snippet below for better understanding.


# Solution
Here is my KeyboardResponder class

# KeyboardResponder
```
import Foundation
import SwiftUI
import Combine

final class KeyboardResponder: ObservableObject {
    private var notificationCenter: NotificationCenter
    @Published private(set) var currentHeight: CGFloat = 0

    init(center: NotificationCenter = .default) {
        notificationCenter = center
        notificationCenter.addObserver(self, selector: #selector(keyBoardWillShow(notification:)), name: UIResponder.keyboardWillShowNotification, object: nil)
        notificationCenter.addObserver(self, selector: #selector(keyBoardWillHide(notification:)), name: UIResponder.keyboardWillHideNotification, object: nil)
    }

    deinit {
        notificationCenter.removeObserver(self)
    }

    @objc func keyBoardWillShow(notification: Notification) {
        if let keyboardSize = (notification.userInfo?[UIResponder.keyboardFrameEndUserInfoKey] as? NSValue)?.cgRectValue {
            currentHeight = keyboardSize.height
        }
    }

    @objc func keyBoardWillHide(notification: Notification) {
        currentHeight = 0
    }
}

```
Then in your Chat View you need to use KeyboardResponder as mentioined below

# ChatView (Only KeyboardResponder related code)

```
import SwiftUI

struct ChatView: View {
    
    @ObservedObject private var keyboard = KeyboardResponder()
    
    init() {
        UITableView.appearance().separatorStyle = .none
        UITableView.appearance().tableFooterView = UIView()
    }
    
    var body: some View {
        NavigationView {
            VStack {
                
            }.navigationBarTitle(Text(DataSource.firstUser.name), displayMode: .inline)
            .padding(.bottom, keyboard.currentHeight)
            .edgesIgnoringSafeArea(keyboard.currentHeight == 0.0 ? .leading: .bottom)
        }.onTapGesture {
                self.endEditing(true)
        }
    }
}
```

