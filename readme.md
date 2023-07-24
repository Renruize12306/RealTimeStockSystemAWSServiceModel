# AWS Service model, streaming the real-time market data
This is a backend and front end application of streaming real-time market data with various AWS services. 

## Prerequisites
* [Installing the AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html)
* [Installing the Docker](https://www.docker.com/)
* [Installing the Julia-1.6.2](https://julialang.org/downloads/oldreleases/)

For mac computers with Apple silicon, Julia 1.6.2 only provide x86_64 aichitecture, hence rosetta should also be installed to run x86_64 Julia.

## Backend end services 
### Create an IAM role for administrative access
This allows us to access and modify the AWS resources through local machine as an [administrative user](https://docs.aws.amazon.com/streams/latest/dev/setting-up.html).

Dorckefile and ./.aws folder have $AWS_ACCESS_KEY_ID and $AWS_SECRET_ACCESS_KEY parameters. Make sure to add those environmental variables to access the access and modify the AWS resources.

### Build and deploy AWS SAM (Serveless Application Model)
```bash
sam build
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

### Configure AWS CloudFormation, deploy and manage the application on AWS services.
```bash
# CDK initialization
mkdir cdk-ts
cd cdk-ts
cdk init app --language typescript
cd CDK
npm install
cdk deploy
```

Open AWS appsync console, we can export schema, we then use this schema add to client folder

We generate query API for JavaScript
```
amplify add codegen --apiId XXXXXXXXXXX
```

After we setup the AWS Cloudformation, we could then running the script to stream the data to the aws services, please refer to streamWSData.jl about how we stream the data.

## Front end services

Once the backend application has been configured, we could then run front end application in ./client folder.
Make sure to change the AppSync configuration before running.