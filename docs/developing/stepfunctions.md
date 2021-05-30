# Step Functions

## Overview

- Model workflows as state machines.
- Serverless workflows to orchestrate lamba functions.
- Support sequence, parallel, conditions, timeouts, error handling.
- Integrates with EC2, EC2, On-Prem Servers, API gateway etc.
- Maximum execution time is 1 year.
- Supports human/manual approval feature.
- Useful for order fulfillment, data processing (ETL), web applications (payment processing).

### States

| State        | Description                                                                                                                   |
|--------------|-------------------------------------------------------------------------------------------------------------------------------|
| Task         | Do some work. Invoke an AWS service, run an activity. Activity will poll the step function for work, and return the results.  |
| Choice       | Test for a condition to send to a branch.                                                                                     |
| Fail/Succeed | Stop execution with fail/success.                                                                                             |
| Pass         | Pass input to an output, or inject some fixed data without doing any work.                                                    |
| Wait         | Wait until a specified date/time, or delay for an amount of time.                                                             |
| Map          | Dynamically iterate over steps.                                                                                               |
| Parallel     | Being parallel branches of execution.                                                                                         |

## Error Handling

The default failure behaviour is to fail the entire execution if a state reports an error.

An error can occur due to:

  - Task failures.
  - Machine definition issues (no matching rule etc).
  - Transient issues (network partition event).

Try to include data in the error messages to help with troubleshooting.

### Pre-defined Error Codes

There's four pre-defined error codes that can be returned -

| Error Code         | Description                                                      |
|--------------------|------------------------------------------------------------------|
| States.ALL         | Catch all errors that occur inside the lambda function.          |
| States.Timeout     | Task ran longer than TimeoutSeconds, or no heartbeat received.   |
| States.TaskFailed  | Execution failure.                                               |
| States.Permissions | Insufficient privileges to execute the code.                     |

### Retrying

Applies to tasks, or parallel state. A task can use multiple retry methods that are evaluated in
order.

There's several parameters that can be used to modify how often a retry is attempted, how many
times to retry before giving up, and a back off rate to increase the interval between each
subsequent retry.

| Method         | Description                                                          |
|----------------|----------------------------------------------------------------------|
| IntervalSecond | Wait a set number of seconds and try again.                          |
| MaxAttempts    | Retry immediately and give up after X attempts (default is 3).       |
| BackoffRate    | Wait an exponentially increasing amount of time before trying again. |


???+ example "Example"
        ``` json
        "HelloWorld": {
          "Type": "Task",
          "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:FUNCTION_NAME",
          "Retry": [
            {
              "ErrorEquals": ["CustomError"],
              "IntervalSeconds": 1,
              "MaxAttempts": 2,
              "BackoffRate": 2.0
            },
            {
              "ErrorEquals": ["States.TaskFailed"],
              "IntervalSeconds": 30,
              "MaxAttempts": 2,
              "BackoffRate": 2.0
            },
            {
              "ErrorEquals": ["States.ALL"],
              "IntervalSeconds": 5,
              "MaxAttempts": 5,
              "BackoffRate": 2.0
            }
          ],
          "End": true
        }
        ```

### Catching Errors

Applies to Tasks or parallel steps, and has two attributes to control what errors to catch,
what the next step should be, and what to include in the result

| Attribute   | Description                                                 |
|-------------|-------------------------------------------------------------|
| ErrorEquals | Match a specific type of error.                             |
| Next        | Proceed to next step.                                       |
| ResultPath  | Way to include the input, into the output of the next task. |

???+ example "Example"
        ``` json
        "HelloWorld": {
          "Type": "Task",
          "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:FUNCTION_NAME",
          "Catch": [
            {
              "ErrorEquals": ["CustomError"],
              "Next": "CustomErrorCallback"
            },
            {
              "ErrorEquals": ["States.TaskFailed"],
              "Next": "ReservedTypeCallback"
            },
            {
              "ErrorEquals": ["States.ALL"],
              "Next": "NextTask",
              "ResultPath": "$.error"
            }
          ],
          "End": true
        },
        "NextTask": {
          "Type": "Pass",
          "Result": "This is a fallback from a reserved error code.",
          "End": true
        },
        "CustomErrorFallback": {
          "Type": "Pass",
          "Result": "This is a fallback from a custom lambda function exception.",
          "End": true
        }
        ```

### Standard Step Functions

- Max duration of 1year.
- Execution start rate up to 2,000/sec.
- State transition rate over 4,000/sec per account.
- Priced per state transition.
- Can be listed and described with step function APIs, and visually debugged through the console.
- Can be inspected in CloudWatch Logs by enabling logging on the state machine.
- Exactly-once workflow execution.

### Express Step Functions

- Max duration of 5mins.
- Execution start rate up to 100,000/sec.
- State transition rate is unlimited.
- Prices by number of executions run, their duration, and memory consumption (cheaper).
- Can be inspected in CloudWatch Logs by enabling logging on the state machine.
- At-least-once workflow execution.
