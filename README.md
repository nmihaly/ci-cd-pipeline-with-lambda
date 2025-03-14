# Skills Assessment 1 - CI/CD Pipeline with AWS Lambda

## Instructions


You are tasked with deploying a code pipeline that automates the creation of new versions of an AWS Lambda function when code is committed to a GitHub repository. A diagram of the solution is provided.

The deployment should include:

- A GitHub repository  for the `lambda_function.py` and `buildspec.yml` files.
- AWS CodeBuild - used for building the deployment package for lambda (`lambda_function.zip`) and running post_build commands to publish a new version of the function.
- AWS CodePipeline - create an AWS CodePipeline to automate the build process.
- AWS Lambda - the pipeline updates the code in the Lambda function.

***Any Python code can be used for the Lambda function***

### Guidelines

- Store your function code in a `lambda_function.py` file in the repository.
- Ensure you add the required permissions for the CodeBuild service role.
- Use the buildspec.yml file code below.

```yml
version: 0.2

phases:
  install:
    runtime-versions:
      python: 3.11
  build:
    commands:
      - echo 'Building...'
      - zip -r lambda_function.zip lambda_function.py
  post_build:
    commands:
      - echo 'Build completed.'
      - aws lambda update-function-code --function-name MyDemoLambda --zip-file fileb://lambda_function.zip
      - echo 'Waiting for 30 seconds before publishing version...'
      - sleep 30
      - aws lambda publish-version --function-name MyDemoLambda

artifacts:
  files: lambda_function.zip
```


## Success Validation

Upon successful completion, when you edit the Lambda code in the `lambda_function.py` file in the repository and commit the changes the pipeline should automate the release of a new version of the function with the updated code.
