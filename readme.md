# AWS Service model, streaming the real-time market data

## Prerequisites
* [Installing the AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html)
* [Installing the Docker](https://www.docker.com/)
* [Installing the Julia-1.6.2](https://julialang.org/downloads/oldreleases/)

For mac computers with Apple silicon, Julia 1.6.2 only provide x86_64 aichitecture, hence rosetta should also be installed to run x86_64 Julia.

## 
Running docker

```bash
sam build
```
Folder ".aws-sam" will be generated

```bash
sam deploy --guided
```

We need to create constant.jl file to configure the some credientails and the ticker we want to subscribe. In this case, aggregate price data for Bitcoin USD using the polygon.io web socket cryptocurrency endpoint.
```
AUTH_PARAM="key from polygon"

event_data = "{\"input_json\":[
    {\"action\":\"auth\",\"params\":\"$AUTH_PARAM\"},
    {\"action\":\"subscribe\",\"params\":\"XA.X:BTC-USD\"}
    ]}"
# this is acquired from the Polygon.io -> dashboard -> API Keys
```


## Configure AWS CloudFormation, deploy and manage the application to AWS services.
```bash
mkdir cdk-ts
cd cdk-ts
cdk init app --language typescript
cd CDK
npm install
cdk deploy
```

Open AWS appsync console, we can export schema add to client folder

We generate query API for JavaScript
```
amplify add codegen --apiId XXXXXXXXXXX
```

After we setup the AWS Cloudformation, we could then running the script to stream the data to the aws services, please refer to streamWSData.jl how we stream the data.