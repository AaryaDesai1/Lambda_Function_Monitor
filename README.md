# Lambda_Function_Monitor

## Objective:
This mini-project was aimed at creating a simple lambda function and implementing AWS X-Ray to monitor the function. This was an extension of a lambda function previously made to read a local csv file which was then zipped, packaged and uploaded to AWS Lambda. 

## Prerequisites:
- AWS CLI
- AWS X-Ray
- AWS Lambda
- AWS IAM User 

## AWS IAM User Policy:
For this project, I used the same policies as the previous project. The policies are as follows:
- AWSLambdaFullAccess
- AmazonS3FullAccess
- AmazonDynamoDBFullAccess
- AmazonCloudFormationFullAccess
- APIGatewayInvokeFullAccess
Additionally, I also added the following to allow for monitoring and logging: 
- AWSXRayFullAccess

## AWS X-Ray:
AWS X-Ray is a service that collects data for requests that an application serves, and provides tools to view, filter, and gain insights into that data to identify issues and opportunities for optimization. To implement X-Ray, I had to add the following dependencies to the Cargo.toml file:
```rust
[dependencies]
lambda_http = "0.9.2"
lambda_runtime = "0.9.1"
tokio = { version = "1", features = ["macros"] }
tracing = { version = "0.1", features = ["log"] }
tracing-subscriber = { version = "0.3", default-features = false, features = ["env-filter", "fmt"] }
csv = "1.1.6"
rusoto_core = "0.47.0"
rusoto_logs = "0.47.0"
rusoto_xray = "0.47.0"
chrono = "0.4.19"
rand = "0.8.5"
uuid = {version = "0.8.2", features = ["v4"]}
```
A lot of the [main.rs]() also had to be changed to include the tracing and xray dependencies as well as the necessary code to implement the tracing and xray.

## Testing the Lambda Function:
1. After writing up your code, you can see if your lambda function works by running `cargo lambda watch` in the terminal. 
<img width="592" alt="image" src="https://github.com/AaryaDesai1/AWS_Lambda_Function/assets/143753050/e80edc39-0cfb-4ebb-893c-4368e9f230aa">

This will run the function locally and you can test it by sending a request to the local endpoint. For me, I could copy http://[::]:9000/ into my browser and see if the function worked. The output looked like this: 
![WhatsApp Image 2024-02-09 at 11 25 24 AM](https://github.com/nogibjj/aad64_Pandas-Script/assets/143753050/c890fad5-38fa-4084-861b-e2dbd7fdf187)

2. After testing the function locally, you need to build the function and upload it to AWS Lambda. You can do this by running `cargo lambda build`.
3. Since I am accessing a local csv, I also needed to deal with zipping and packaging the csv file. I did this by running the following commands in the terminal:
```bash
aws lambda update-function-code --function-name your_lambda_function_name --zip-file fileb://target/lambda/<your-lambda-function-name>/bootstrap.zip
```
This allowed me to upload the zipped file to AWS Lambda.

## Monitoring the Lambda Function:
After the function is uploaded to AWS Lambda, you can monitor the function by going to the AWS X-Ray console. Here, you can see the traces and the performance of the function.


