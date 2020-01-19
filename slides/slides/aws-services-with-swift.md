## AWS services with Swift

Arek Turlewicz
(アレック）

@a_turl

---

## 2020

  - everyone is using Swift Package Manager
  - we can share our code easly between MacOS and iOS applications

## Reality

  - https://github.com/aws-amplify/aws-sdk-ios/issues/313
  - https://github.com/awslabs/aws-mobile-appsync-sdk-ios/issues/288

## Current State

  - Carthage (it takes ages to install dependencies)
  - Cocoapods (it will make mess in your xcode configuration)

## Luckly we have alternative choice

  - https://github.com/swift-aws/aws-sdk-swift

--

  ```
    dependencies: [
        .package(url: "https://github.com/swift-aws/aws-sdk-swift.git", from: "4.0.0")
    ],
  ```

--
## you can install only pieces you need

  ```
    targets: [
      .target(name: "AppWithAWSResources", dependencies: ["S3", "Lambda"]),
    ]
  ```



## Links
- https://swift.org/download/
- https://www.digitalocean.com/community/tutorials/how-to-install-swift-and-vapor-on-ubuntu-16-04
- https://medium.com/@vsemenchenko/compiling-swift-source-270691f31045
- https://swift.org/package-manager/
- https://github.com/serverless/examples
- https://medium.com/capital-one-tech/serverless-computing-with-swift-f515ff052919
- https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks
