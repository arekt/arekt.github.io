## Lambda for Rubyist

@a_turl

MediWeb, Inc.
---

## Little bit definitions

### Lambda

- is a kind of function
- it can respond to events
- it can generate artefacts

---

## Hello World

```
handler.hello => {
  callback(null, sucess_resoponse)
}
```
---

## Ruby is not supported

We can write Lambda code only in one of the languages:
- Node.js (JavaScript)
- Python
- Java (Java 8 compatible)
- C# (.NET Core)
- Go

---

We can call Lambda function from ruby

### aws-sdk-lambda gem

```
require 'aws-sdk-lambda'

@client = Aws::Lambda::Client.new(region: 'ap-northeast-1')

@result = @client.invoke({
  function_name: "my-lambda",
  payload: "some payload here"
})

@result.on_success do |data|
  puts data.payload.read
end
```

### REST request to API Gateway

---

## We already needs to deal with javascript

--

## Who loves asset pipeline?
--

## We all love Ember, React and other javascript frameworks...

---

## Lets add ruby support ;)

* MRuby

--

* JRuby

--

* CRuby

---

### Limitations of AWS Lambda

* Max execution time is 5sec.

* Max code size is 50MB in compressed zip

## Running ruby with exec

```
ma1:cruby arekt$ time curl https://pnx9n71ue5.execute-api.ap-northeast-1.amazonaws.com/dev/ruby/hello
2018-03-04T12:43:24+00:00

real	0m1.367s    <---------- lambda was cold
user	0m0.041s
sys	0m0.013s
```

--

```
ma1:cruby arekt$ time curl https://pnx9n71ue5.execute-api.ap-northeast-1.amazonaws.com/dev/ruby/hello
2018-03-04T12:43:10+00:00


real	0m0.643s     <---------- lambda after warm up
user	0m0.041s
sys	0m0.012s
```
--

In both cases memory level was set 1024MB

## Running ruby as a daemon

--
* waiting until daemon is already
* passing data between lambda handler and ruby

---

## How to deploy our code?
- use AWS console in the browser
- aws cli / aws sdk

---
  or ...
---

### Serverless

- AWS CloudFormation

```
npm install serverless
```

## deploy
```
sls deploy
```
--

## call function with logging the output

```
sls invoke -f hello -l
```
--

## Deploy only lambda code

```
sls deploy function -f hello
```
--

## Cron example with serverless

```
service: hello

provider:
  name: aws
  runtime: nodejs4.3

functions:
  hello:
    handler: handler.hello
```

## Lets run our lambda every 1 min

```
functions:
  hello:
    handler: handler.hello
    events:
      - schedule: rate(1 minute)
```

## Lets add http api endpoint
```
functions:
  hello:
    handler: handler.hello
    events:
      - http:
          path: ruby/hello
          method: get
```
---

## Reducing code bundle size

### Reducing Lambda size

https://github.com/phusion/traveling-ruby/blob/master/REDUCING_PACKAGE_SIZE.md

## Remove all binary gems from dependencies

1. What my app is really using
2. Converting gem with binary dependencies to lambda endpoint

---

## Links

- https://github.com/mruby/mruby
- https://docs.aws.amazon.com/sdk-for-ruby/v3/api/index.html
- https://github.com/serverless/examples
- https://github.com/phusion/traveling-ruby
- https://github.com/Shopify/bootsnap
