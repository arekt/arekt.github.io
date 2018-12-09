## Cloud Swift with AWS


Arek Turlewicz
(アレック）

@a_turl

Code transfigurator at MediWeb, Inc.

---

## Introduction

  * AWS Lambda
  * Server-side Swift
  * make them work together

---

## What is AWS Lambda?

![lambda-in-the-cloud](lambda-1a.png)

---

## What is AWS Lambda?

  - it is a function <!-- .element: class="fragment" -->
( It takes input and returns output ) <!-- .element: class="fragment" -->

  - it can be triggered by events <!-- .element: class="fragment" -->

  - it can generate artifacts <!-- .element: class="fragment" -->

  - it can call other functions <!-- .element: class="fragment" -->

  - it supports many programming languages like Java, NodeJS, Python but not Swift <!-- .element: class="fragment" -->

---

## What is AWS Lambda?

![lambda-in-the-cloud2](lambda-1b.png)

---

## Why to use AWS Lambda?

* is part of the huge AWS infrastructure <!-- .element: class="fragment" -->
 IoT, AI, File storege, Databases, Message Services, Network components, Servers <!-- .element: class="fragment" -->
* having stuff in the Cloud make it easier to share with others <!-- .element: class="fragment" -->
* with the right tools, maintenance and development is easy <!-- .element: class="fragment" -->

---

## Utils

* AWS Api in many languages (including Swift)
* Serverless - tool to manage AWS infrastructure

---

```
npm install -g serverless
open handler.js
open serverless.yaml
serverless deploy
```

---

## Lambda handler in Javascript

```
exports.hello = (event, context, callback) => {
  callback(null, "Hello World")
}
```


---

## Cron example with Serverless

```
service: hello

functions:
  hello:
    handler: handler.hello
    events:
      - schedule: rate(1 minute)
```

---

## Api Gataway (you can run Lambda in the browser)

```
    events:
      - http:
          path: ruby/hello
          method: get
```

---

## Why Swift?

(Do I need to ask this question?)

* Swift is awesome language <!-- .element: class="fragment" -->
* Client and Backend in the same language is easier to develop <!-- .element: class="fragment" -->
* A lot of existing libraries and frameworks <!-- .element: class="fragment" -->
* Super easy to wrap existing c libraries <!-- .element: class="fragment" -->
* Swift Package Manager is great <!-- .element: class="fragment" -->

---

## Creating project using Swift Package Manager

```
$ mkdir cmdFoo
$ cd cmdFoo
$ swift package init --type executable
```

---

```
$ swift build
$ swift run
Hello, World!
```
and it will work same on your Linux box too :)

---

## Generating xcodeproj

```
$ swift package generate-xcodeproj
open cmdFoo.xcodeproj
```

---

## So how to get swift code to run on Lambda?

* find Linux Box ( Docker, Virtual Machine, EC2 or standalone machine )
* install swift
* compile Swift code
* write wrapper to run binary
* upload using api or serverless

---

![lambda-in-the-cloud](lambda-2a.png)

---

## Smooth deploy

Using 携帯 or pocket wifi, uploading Megabytes takes little bit time.

If you compile code in the Cloud, you upload only source code changes.

---

* post-receive Git hook

```
ubuntu@in-the-cloud:~$ cat cmdFoo.git/hooks/post-receive
#!/bin/bash
while read oldrev newrev ref
do
  work_dir=/home/ubuntu/workspace/cmdFoo
  bare_git_root=/home/ubuntu/cmdFoo.git
  git --work-tree=${work_dir} --git-dir=${bare_git_root} \
  　checkout -f ${newrev}
  cd ${work_dir}
  ./deploy.sh
done
```

---

## Binary compatibility problem

### Did not work
* static linking (anyone can share experience?) <!-- .element: class="fragment" -->
* using close as possible system to running lambda environment (AWS AMI) <!-- .element: class="fragment" -->

### Worked
* use Ubuntu Linux and use sorcery to convince Lambda to run it
  (only Ubuntu is officially supported by Swift) <!-- .element: class="fragment" -->

---

## Compile Swift program

### deploy.sh
```
#!/bin/bash
set -e
export PATH=/opt/swift-4.1/usr/bin:"${PATH}"
swift build --static-swift-stdlib -c release -Xlinker -lhiredis -Xcc -D_FILE_OFFSET_BITS=64 -Xcc -I/usr/include/hiredis
rm -rf lib
mkdir -p lib
mkdir -p bin
./find_lib_dependencies.sh ./.build/x86_64-unknown-linux/release/cmdFoo | xargs -i% cp % lib/
cp ./.build/x86_64-unknown-linux/release/cmdFoo bin/
cp /lib/x86_64-linux-gnu/ld-2.23.so lib/
cp /lib64/ld-linux-x86-64.so.2 lib/
./upload.sh
```

---

### Linker options

-Xlinker -lhiredis

---

### C Compiler options

-Xcc -D_FILE_OFFSET_BITS=64 -Xcc -I/usr/include/hiredis

---

## We need to find all dependencies

### find_lib_dependencies.sh
```                                                                                      [22:48:39]
#!/bin/bash
set -e
[ -x $1 ] && \
ldd $1 | grep "\.so.* => /" | cut -d " " -f 3
```

---

## Javascript wrapper to run swift binary

```
const spawnSync = require("child_process").spawnSync

exports.hello = (event, context, callback) => {
  const command = "/var/task/lib/ld-linux-x86-64.so.2"
  const childObject = spawnSync(command,
    ["--library-path", "/var/task/lib", "./bin/cmdFoo"],
    {input: JSON.stringify(event)})

  let response = {
    statusCode: 200,
    body: childObject.stdout.toString("utf8")
  }
  callback(null, response)
};
```

---

## Oh wait...
### now AWS supports Ruby

```
def hello(event:, context:, callback:)
  command = "/var/task/lib/ld-linux-x86-64.so.2 \
    --library-path /var/task/lib ./bin/cmdFoo"
  {
    statusCode: 200,
    body: IO.popen(command) { |f| f.puts JSON.generate(event) }
  }
end
```

---

## Example swift code

```
import Foundation
import SwiftyJSON

let inData = FileHandle.standardInput.availableData
let event: JSON = try! JSON(data: inData)

var jsonDictionary: JSON = [
    "event": [
      "queryStringParameters": event["queryStringParameters"],
      "httpMethod": event["httpMethod"],
      "path": event["path"],
print(jsonDictionary.rawString()!)
```

---

## Demo

---

## Links
- https://swift.org/download/
- https://www.digitalocean.com/community/tutorials/how-to-install-swift-and-vapor-on-ubuntu-16-04
- https://medium.com/@vsemenchenko/compiling-swift-source-270691f31045
- https://swift.org/package-manager/
- https://github.com/serverless/examples
- https://medium.com/capital-one-tech/serverless-computing-with-swift-f515ff052919
- https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks
