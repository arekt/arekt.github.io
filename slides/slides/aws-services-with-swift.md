## AWS services with Swift

Arek Turlewicz
(アレック）

@a_turl
https://github.com/arekt/

---

before we start

---

### So how running Swift on AWS Lambda is going?

- https://github.com/fabianfett/swift-lambda-runtime
- https://docs.aws.amazon.com/lambda/latest/dg/runtimes-api.html

---

## Year 2020

  - everyone is using Swift Package Manager
  - we can share our code easly between MacOS, Server Swift and iOS applications

---

## Reality

  We still have a lot of libraries with objective-C dependencies

  - Carthage (it takes ages to install dependencies)
  - Cocoapods (it will make mess in your xcode configuration)

---

### How to connect to AWS Resources

  - AWS resources behind our custom API (HTTP, REST, GraphQL)
  - call lambda function ( we can mix with previous step )
  - using Swift library, so it will work on MacOS and iOS
  - AWS library already talk to rest API, so we could talk to API directly
  (require request signature)

---

### Authentication/Authorization Services

  ## Cognito User Pools and Cognito Identity Pools

  After successfully
  authenticating a user,
  Amazon Cognito issues
  JSON web tokens (JWT)                   <--- Cognito User Pool
  that you can use to secure
   and authorize access to your own APIs,
  or exchange for AWS credentials.        <--- Cognito Identity

---

  - if you are using traditional HTTP based API probably you just need Cognito User Pool

  - if you want to access AWS services like S3, or DynamoDB directly, from Swift application you will have to exchange token for AWS Credentials using Cognito Identity

  - in some cases it make sense to create special unauthenticated user that will have
  permissions to access AWS resources

---

### Using aws-sdk-swift library

  - https://github.com/swift-aws/aws-sdk-swift

---

Package.swift

```
dependencies: [
  .package(url: "https://github.com/swift-aws/aws-sdk-swift.git",
    from: "4.0.0")
],
```

---

  you can install only pieces you need
  and reduce compilation time

```
targets: [
  .target(name: "AppWithAWSResources",
    dependencies: ["S3", "Lambda"]),
]
```

---

### Examples

---

#### Accessing S3 Objects

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

and you can use it like:

```
  var storage: Storage?

  init() {
    storage = Storage(bucket: "yourBacketName")
  }

  func listS3Folder() {
    storage?.list(path: self.path) { files in
      self.fileNames = files.map { file in
        file.key ?? ""
      }
    }
  }
```

---

#### GraphQL queries through Lambda

```
import Lambda
import Foundation

struct Root<T:Codable>: Codable {
  var data: T
}

```

--

```
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
    ...
```

--

```
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

```
struct BookReaderView: View {
  @State var text: String = ""
  var prevPage = LambdaRequest<PrevPageResponse>(query: "mutation{prevPage{page}}")
  var openPage = LambdaRequest<BookResponse>(query: "{book{page}}")
  var nextPage = LambdaRequest<NextPageResponse>(query: "mutation{nextPage{page}}")

```

--

```
  var body: some View {
    VStack {
      TextEditorView(text: $text)
      HStack{
        Button(action: {
          self.prevPage.execute { (response: PrevPageResponse) in
            self.text = response.prevPage.page
          }
        }) { Text("Previous") }
```

---

#### Processing messages in SQS Queues

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
```

--

```

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
```

--

```

  func getMessages(successCallback: @escaping (_ text: String) -> Void) {
    let receiveMessageRequest = SQS.ReceiveMessageRequest(
      maxNumberOfMessages: 1,
      messageAttributeNames: ["all"],
      queueUrl: url,
      waitTimeSeconds: 1
    )
    let promise = sqs.receiveMessage(receiveMessageRequest)
    promise.whenSuccess{ response in
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

---

## Can we use aws-sdk-swift library to authenticate Cognito User?

---

  hmm...

--

  ...sorry was watching The Wither recently

---

```
  import CognitoIdentityProvider
  var cognitoIdentityProvider = CognitoIdentityProvider(region: .apnortheast1)
  let clientId = "put clientid here"
  let loginRequest = CognitoIdentityProvider.InitiateAuthRequest(
    authFlow: .userSrpAuth,
    authParameters: ["USERNAME": username, "SRP_A": srp_a],
    clientId: clientId)
  let promise = cognitoIdentityProvider.initiateAuth(loginRequest)
```

---

  What is SRP_A ?

---

  The Secure Remote Password protocol (SRP) is an augmented password-authenticated key agreement (PAKE) protocol, specifically designed to work around existing patents.

---

  Like all PAKE protocols, an eavesdropper or man in the middle cannot obtain enough information to be able to brute force guess a password without further interactions with the parties for each guess. Furthermore, being an augmented PAKE protocol, the server does not store password-equivalent data. This means that an attacker who steals the server data cannot masquerade as the client unless they first perform a brute force search for the password.


---

### There is few SRP libraries for Swift

  https://github.com/Bouke/SRP
  https://github.com/flockoffiles/SwiftySRP


---

## Answer:

  We can use Cognito using Swift, but it's little bit harder
  then with AWS iOS Library.

---

## Other Interesting AWS services/libraries

---

### Amplify and AppSync

---

### AWS AppSync is an enterprise-level, fully managed GraphQL service with real-time data synchronization and offline programming features.

  https://aws-amplify.github.io/docs/ios/start?ref=amplify-iOS-btn
  https://docs.aws.amazon.com/appsync/latest/devguide/welcome.html

---

### CDK

  https://docs.aws.amazon.com/cdk/latest/guide/home.html

  There is ticket with feature request for Swift support:

  https://github.com/aws/aws-cdk/issues/549

---

## Links
- https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html
- https://github.com/fabianfett/swift-lambda-runtime
- https://docs.aws.amazon.com/lambda/latest/dg/runtimes-api.html
- https://github.com/swift-aws/aws-sdk-swift
- https://swift.org/package-manager/
- https://itnext.io/aws-amplify-react-native-authentication-full-setup-7764b452a138
- https://en.wikipedia.org/wiki/Secure_Remote_Password_protocol
