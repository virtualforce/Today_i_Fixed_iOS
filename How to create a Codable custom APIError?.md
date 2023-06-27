# Problem
In a good architecture we usually write a NetworkManager to handle all the API calls and for that we need a Codable custom APIError which we can use in case of failure.


# Environment
N/A


# How you fix it
=> I created a structure conforming to **Codable** & **Error** protocols with the required properties/keys. 


# Solution
As I mentioned above I created a structure with two init methods as per the requirements.
Please note: You can add/rename keys/properties as per your requirements.
I am providing the helping Code below:


# APIError
```
import Foundation

// MARK: APIError
struct APIError: Codable, Error {
    var status, statusTitle: String?
    var result: String?
    var message, messageCode: String?
    var errorCode: Int?
    
    init(status: String?, statusTitle: String?, message: String?, messageCode: String?) {
        self.status = status
        self.statusTitle = statusTitle
        self.message = message
        self.messageCode = messageCode
        self.errorCode = Int(messageCode ?? "")
    }
    
    init(status: String?, statusTitle: String?, message: String?, messageCode: String?, errorCode: Int?) {
        self.init(status: status, statusTitle: statusTitle, message: message, messageCode: messageCode)
        self.errorCode = errorCode
    }
    
}

// MARK: Usage

// Create insstance using JSONDecoder()
let decoder = JSONDecoder()
let apiError = try decoder.decode(APIError.self, from: response.data) // response.data is json response data from server etc.
print(apiError)

// Create insstance using init methods
let apiError = APIError.init(status: Constants.Error, statusTitle: Constants.Error, message: Constants.GeneralFailureMessage, messageCode: "\(response.statusCode)")
print(apiError)

```
