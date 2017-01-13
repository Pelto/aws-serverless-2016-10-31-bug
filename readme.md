# Example of SAM bug

For some reason it is not possible to add the `CodeUri` property as a parameter
in the `AWS::Serverless::Function`. Steps to reproduce the issue:

```bash

export BUCKET_NAME=<mybucket>

zip -r example.zip index.js

aws s3 cp example.zip s3://$BUCKET_NAME/

aws cloudformation deploy --stack-name example --template-file lambda-example.sam.yaml --parameter-overrides CodeUri=s3://$BUCKET_NAME/example.zip

````

The above example will generate the following error message:

```
Failed to create the changeset: Waiter ChangeSetCreateComplete failed: Waiter encountered a terminal failure state Status: FAILED. Reason: Transform AWS::Serverless-2016-10-31 failed with: Invalid Serverless Application Specification document. Number of errors found: 1. Resource with id [HelloWorldFunction] is invalid. 'CodeUri' is not a valid S3 Uri of the form "s3://bucket/key" with optional versionId query parameter.
```

If we instead specify the `CodeUri` as a constant in our template everything
works as expected. It is only when we try to add the `CodeUri` property as a
parameter.