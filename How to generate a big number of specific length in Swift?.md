# Problem
Need a big random number of a specific length in Swift.


# Environment
N/A


# How you fix it
=> This problem could be solved using the .random() function available in Swift but in that case, you need to provide the range. 
If you need a number with 19 digits you need to provide an upper limit of range with 19 digits number which seems a hassle.
I devised two solutions for this problem which I am sharing below


# Solution
1) Use UUID Strings and filter out all the numbers until you get the desired length
2) Start from an empty and attach random digits from the range 1-9 until you get the desired length. But please note in this case your loop will iterate n times where n = digits lenght.

# 1) Using UUID Strings
```
import Foundation

func generateBigRandomNumber(digits: Int = 19) -> UInt64 {
    var randomDigits = ""

    while randomDigits.count < digits {
        let uuid = UUID().uuidString.replacingOccurrences(of: "-", with: "")
        randomDigits += uuid.filter { $0.isNumber }
    }

    randomDigits = String(randomDigits.prefix(digits))
    return UInt64(randomDigits) ?? 0
}
```

# 1) Using Strings and adding 1 random digit at a time
```
import Foundation

func generateBigRandomNumberOf(digits: Int) -> String {
    
    var bigRandomNumber = String()
    for _ in 1...digits {
       bigRandomNumber += "\(Int.random(in: 1...9))"
    }
    return bigRandomNumber
}

```
