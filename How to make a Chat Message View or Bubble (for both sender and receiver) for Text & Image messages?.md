# Problem
Make a Chat Message View/Bubble (a single one for both Sender & Receiver) instead of two, to handle Text and Image messages.


# Environment
- SwiftUI
- Minimum iOS version 14.0


# How you fix it
=> I will focus only on the Chat Message View / Bubble functionality. I have made a **MessageView** and a **ContentMessageView** to achieve this functionality. 
My main ChatView is using the MessageView to display messages which eventually is using the ContentMessageView.


# Solution
The code snippets are provided below to help out in implementing the functionality.

# MessageView
```
import SwiftUI

struct MessageView : View {
    
    var currentMessage: Message
    
    var body: some View {
        HStack(alignment: .bottom, spacing: 15) {
            if !currentMessage.user.isCurrentUser {
                Image(currentMessage.user.avatar)
                .resizable()
                .frame(width: 40, height: 40, alignment: .center)
                .cornerRadius(20)
            } else {
                Spacer()
            }
            ContentMessageView(contentMessage: currentMessage.content, messageType: currentMessage.messageType,
                               isCurrentUser: currentMessage.user.isCurrentUser)
        }.padding()
    }
}
```

# ContentMessageView
```
import SwiftUI

struct ContentMessageView: View {
    
    var contentMessage: String
    var messageType: MessageType = .text
    var isCurrentUser: Bool
    
    var body: some View {
        
        if messageType == .text {
            
            Text(contentMessage)
                .padding(10)
                .foregroundColor(isCurrentUser ? Color.white : Color.black)
                .background(isCurrentUser ? Color("GoldenColor") : Color(UIColor(red: 240/255, green: 240/255, blue: 240/255, alpha: 1.0)))
                .cornerRadius(10)
            
        } else {
            
            Image(contentMessage)
                .resizable()
                .frame(minWidth: 250, maxWidth: 250, minHeight: 300, maxHeight: 300)
        }
    }
}
```


# Models
Code snipets for the models are provided below

## User
```
import Foundation

struct User: Hashable {
    var name: String
    var avatar: String
    var isCurrentUser: Bool = false
}
```

## Message

```
import Foundation

enum MessageType {
    case text
    case image
}

struct Message: Hashable {
    var content: String
    var messageType: MessageType = .text
    var user: User
}
```


