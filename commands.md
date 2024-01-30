# sentiment-prediction

This project contains source code and supporting files for a serverless application that you can deploy with the SAM CLI. It includes the following files and folders.

- hello_world - Code for the application's Lambda function and the classification model in *.pickle* format.
- events - Invocation events that you can use to invoke the function.
- tests - Unit tests for the application code. 
- template.yaml - A template that defines the application's AWS resources.

The application uses several AWS resources, including Lambda functions and an API Gateway API. These resources are defined in the `template.yaml` file in this project. You can update the template to add AWS resources through the same deployment process that updates your application code.


## What is SAM CLI?

The Serverless Application Model Command Line Interface (SAM CLI) is an extension of the AWS CLI that adds functionality for building and testing Lambda applications. It uses Docker to run your functions in an Amazon Linux environment that matches Lambda. It can also emulate your application's build environment and API.

To use the SAM CLI, you need the following tools.

* SAM CLI - [Install the SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
* [Python 3 installed](https://www.python.org/downloads/)
* Docker - [Install Docker community edition](https://hub.docker.com/search/?type=edition&offering=community)

## Using SAM CLI local to run the lambda function locally

### 1. Install the AWS SAM CLI 

Run the following:

`$ brew install aws-sam-cli`

Verify the installation:

`$ sam --version`

### 2. Initialize a new serverless application using the AWS SAM CLI

cd to a starting directory.

Run the following at the command line:

`$ sam init`

The AWS SAM CLI will guide you through an interactive flow to create a new serverless application. For details refer to [here.](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/using-sam-cli-init.html)

### 3. Build an application

`$ sam build`

Refer to [here](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/using-sam-cli-build.html) for the details.

### 4. Test your application locally

Use the sam local command with any of its subcommands to perform different types of local testing for your application.

`$ sam local <subcommand>`

#### 4.1 Generate AWS service events for local testing

Create a test event in *.json* format in the events folder. Refer [here](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/using-sam-cli-local-generate-event.html) for details.

`$ sam local generate-event`

#### 4.2 Invoke a Lambda function locally 

To initiate a one-time invocation of an AWS Lambda function, from the root directory of your project, run the following:

`$ sam local invoke <options>`

If your application contains more than one function, provide the function’s logical ID. The following is an example:

`$ sam local invoke HelloWorldFunction`

If you want to invoke it with an event --event *\<path>* parameter should be added.

The AWS SAM CLI builds your function in a local container using Docker. It then invokes your function and outputs your function’s response.
However, when I run that command on a Mac OS, even though I already installed Docker I got the following error :

*"Error: Running AWS SAM projects locally requires Docker."*

It was solved by adding DOCKER_HOST environment variable to the beginning of the command:

`$ DOCKER_HOST=unix://$HOME/.docker/run/docker.sock sam local invoke HelloWorldFunction --event events/event.json`


#### 4.3 Start a local HTTP server

Run your AWS Lambda functions locally and test through a local HTTP server host. From the root directory of your project, run the following:

`$ sam local start-api <options>`

In my case, I use the following command:
`$ DOCKER_HOST=unix://$HOME/.docker/run/docker.sock sam local start-api`

- To test the endpoint, you can run below cURL command:

```shell
curl  -X POST \
  'http://127.0.0.1:3000/hello' \
  --header 'Content-Type: application/json' \
  --data-raw '{
  "review":"a wonderful unforgettable movie"
}'
```

- You will get the result below after running the previous command:

```shell
{
  "result": "pos"
}
```