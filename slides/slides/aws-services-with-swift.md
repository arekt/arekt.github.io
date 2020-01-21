## AWS services with Swift

Arek Turlewicz
(アレック）

@a_turl

---

### So how running Swift on Lambda is going?

- https://github.com/fabianfett/swift-lambda-runtime
- https://docs.aws.amazon.com/lambda/latest/dg/runtimes-api.html

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

### How to connect to AWS Resources

  - AWS resources behind our custom API (HTTP, REST, GraphQL)
  - call lambda function ( we can mix with previous step )
  - using swift library, so it will work on MacOS and iOS
  - AWS library already talk to rest API, so we could talk to API directly
  (require sinature)


### AWS Services can be confusing sometimes

  STS and Cognito Identity have similar function:
    retrieve temporary AWS credentials to do some job


--

  Difference between Cognito User Pools and Cognito Identity Pools

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

### Examples

---

## Accessing S3 Objects

```
import SwiftUI
import S3

struct Storage {
  let s3 = S3(region: .useast1 )
  var bucket: String

  func list(path: String, successCallback: @escaping (_ files: [S3.Object]) -> Void) {
    let getObjectRequest = S3.ListObjectsRequest(bucket: bucket, prefix: path)
    let promise = s3.listObjects(getObjectRequest)
    promise.whenSuccess{ response in
      print("success...")
      if let files = response.contents {
        successCallback(files)
      }
    }
    promise.whenFailure{ response in
      print("error... \(response)")
    }
  }

  func get(path: String, successCallback: @escaping (_ text: String) -> Void) {
    let getObjectRequest = S3.GetObjectRequest(bucket: bucket, key: path)
    let promise = s3.getObject(getObjectRequest)
    promise.whenSuccess{ response in
      print("success...")
      if let body = String(data: response.body!, encoding: .utf8) {
        successCallback(body)
      }
    }
    promise.whenFailure{ response in
      print("error... \(response)")
    }
  }
}
```

---

## GraphQL queries through Lambda

```
import Lambda
import Foundation

struct Root<T:Codable>: Codable {
  var data: T
}

struct LambdaRequest<T:Codable> {
  let lambda = Lambda(region: .apnortheast1)
  var variables: [String:String] = [:]
  var query: String

  func execute(onSuccess: @escaping (_ response: T) -> Void) {

    let data = try? JSONSerialization.data(
      withJSONObject: ["body": query, "variables": variables],
      options: [])

    let request = Lambda.InvocationRequest(
      functionName: "gql-api-dev-graphql",
      //invocationType: .requestresponse,
      payload: data
    )
    let promise = lambda.invoke(request)
    promise.whenSuccess{ response in
      print("success...")
      if let body = String(data: response.payload!, encoding: .utf8) {
        print("body: \(body)")
      }
      if let content = try? JSONDecoder().decode(Root<T>.self, from: response.payload!) {
        print("good...")
        onSuccess(content.data)
      }
    }
    promise.whenFailure{ response in
      print("error... \(response)")
    }
  }
}
```

---

## Processing messages in SQS Queues

```
import SwiftUI
import SQS

struct MyMessage {
  var body: String
}

struct MessageQueue {
  let sqs = SQS(region: .apnortheast1)
  var queueName: String = "foo"
  @State var url: String

  func deleteMessage(message: SQS.Message) {
    if let handle = message.receiptHandle {
      let deleteRequest = SQS.DeleteMessageRequest(
        queueUrl: url,
        receiptHandle: handle
      )

      let promise = sqs.deleteMessage(deleteRequest)
      promise.whenSuccess{ response in
        print("message deleted...")
      }
      promise.whenFailure{ response in
        print("error... \(response)")
      }
    }
  }

  func sendMessage(text: String) {
    let sendMessageRequest = SQS.SendMessageRequest(
      messageBody: text,
      queueUrl: url
    )
    let promise = sqs.sendMessage(sendMessageRequest)
    promise.whenSuccess{ response in
      print("message sent")
      print(response.messageId)
    }
    promise.whenFailure{ response in
      print("error... \(response)")
    }
  }

  func getMessages(successCallback: @escaping (_ text: String) -> Void) {
    let receiveMessageRequest = SQS.ReceiveMessageRequest(
      maxNumberOfMessages: 1,
      messageAttributeNames: ["all"],
      queueUrl: url,
      waitTimeSeconds: 1
    )
    let promise = sqs.receiveMessage(receiveMessageRequest)
    promise.whenSuccess{ response in
      print("success... received messages")
      print(response.messages)
      response.messages?.forEach({ message in
        print("message: \(message.body)")
        successCallback(message.body!)
        self.deleteMessage(message: message)
      })
    }
    promise.whenFailure{ response in
      print("error... \(response)")
    }
  }
}

```

### Authentication

  - Use api key
  - Use hardcoded client key and secret
  - Call api for cognito
  - get user oauth token

---

## Links
- https://github.com/fabianfett/swift-lambda-runtime
- https://docs.aws.amazon.com/lambda/latest/dg/runtimes-api.html
- https://github.com/swift-aws/aws-sdk-swift
- https://swift.org/package-manager/
- https://itnext.io/aws-amplify-react-native-authentication-full-setup-7764b452a138
