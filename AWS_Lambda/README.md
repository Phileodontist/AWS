# AWS Lambda
***
Serves as a compute service that lets you run code in response to events via triggers.

## Concepts

### Naming
The Lambda function `handler` name specified at the time you create a Lambda function is derived from the following:
* the name of the file in which the Lambda handler function is located
* the name of the Python handler function

The name reflects the name of the `handler` function and the file where the handler code is stored.

**Note:** If you choose a different name, be sure to change the handler name in the `Runtime settings`

### Runtime
A language-specific environment that runs in an execution environment.

### Event Object
A JSON document that contains data for a Lambda function to process

### Context Object
Provides methods and properties that provide information about the function and execution environment.

## Workflow

When the function handler is invoked, the Lambda runtime passes two arguments to the function handler, an event object and context object.

