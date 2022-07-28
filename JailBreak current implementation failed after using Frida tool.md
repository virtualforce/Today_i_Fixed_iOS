## Problem
JailBreak current implementation failed after using Frida tool.

## How you fix it
Using iOS Security Suite is an advanced and easy-to-use platform security & anti-tampering library written in pure Swift! If you are developing for iOS and you want to protect your app according to the OWASP MASVS standard, chapter v8, then this library could save you a lot of time

## Solution
```
1. pod 'IOSSecuritySuite'
2. After adding IOSSecuritySuite to your project, you will also need to update your main Info.plist. There is a check in jailbreak detection module that uses canOpenURL(_:) method and requires specifying URLs that will be queried.
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>cydia</string>
    <string>undecimus</string>
    <string>sileo</string>
    <string>zbra</string>
    <string>filza</string>
    <string>activator</string>
</array>
3. Jail Break detection and reversed engineer detection code
IOSSecuritySuite.amIJailbroken() 
IOSSecuritySuite.amIReverseEngineered()
```
