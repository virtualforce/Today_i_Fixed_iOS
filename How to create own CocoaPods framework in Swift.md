## Problem
How to create own CocoaPods framework in Swift?

## How you fix it
Being an iOS developer we may have used lot of CocoaPods libraries in our project, and we know how to install them and use their methods. However, have you ever dreamed of contributing to the iOS open-source community by distributing your own pod(CocoaPods library)? Imagine many iOS apps are going to use your awesome library, isn’t that fantastic?
In this Solution, I have mentioned the steps to create your own cocoa pods library.
 

## Solution
```
1. Create new Xcode Project and select Framework in application type. After that implement your code which you want to share with developer community.
2. Push code on Github Public Repository.
3. Tag version and push to repo.       	=> git tag 1.0.0, git push origin —tags
4. Create Pod spec file                       	=> pod spec create PodSpecFileName
5. Update Podspec file 
6. Validate Pod Spec file 			=>  pod spec lint filename.podspec --allow-warnings
7. Push Pod Spec file
8. Optionally add sample project with local podspec path 
9. Register CocoaPod session => pod trunk register abc@gmail.com ‘name’ —description=‘abc desc’
10. Push Pods to cocoa pods => pod trunk push  name.podspec 

```
