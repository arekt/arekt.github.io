## AWS services with Swift

Arek Turlewicz
(アレック）

@a_turl

---

## 2020

  - everyone is using Swift Package Manager
  - we can share our code easly between MacOS/ServerSide Swift App and iOS applications

---

## Reality

  - https://github.com/aws-amplify/aws-sdk-ios/issues/313
  - https://github.com/awslabs/aws-mobile-appsync-sdk-ios/issues/288

---

## Current State

  - Carthage (it takes ages to install dependencies)
  - Cocoapods (it will make mess in your xcode configuration)

---

### Options with using api

  - http andponint
  - call lambda function ( we can mix with previous step )
  - using swift library, so it will work on MacOS and iOS
  - AWS library already talk to rest API, so we could talk to API directly
  (require sinature)

---

###

  TODO: maybe is worth to mention STS


###

  TODO: difference between Cognito Users and Cognito Identity

---

 TODO: Can we use appsync with https://github.com/swift-aws/aws-sdk-swift ??

---


## Using Swift Library

  - https://github.com/swift-aws/aws-sdk-swift

---

  ```
    dependencies: [
        .package(url: "https://github.com/swift-aws/aws-sdk-swift.git", from: "4.0.0")
    ],
  ```

---

## you can install only pieces you need

  and reduce compilation time

  ```
    targets: [
      .target(name: "AppWithAWSResources", dependencies: ["S3", "Lambda"]),
    ]
  ```

---

### Authentication

  - Use api key
  - Use hardcoded client key and secret
  - Call api for cognito
  - get user oauth token

---

## Links

- https://github.com/swift-aws/aws-sdk-swift
- https://swift.org/package-manager/
