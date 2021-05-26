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

- Task failures,machine definition issues (no matching rule etc), transient issues (network partition event).
- Default behaviour is to fail the entire execution if a state reports an error.
- There's three ways to retry execution when an error is encountered:
  - Retry: IntervalSecond (wait a set number of seconds and try again)
  - Retry: MaxAttempts (retry immediately and give up after X attempts)
  - Retry: BackoffRate (wait an exponentially increasing amount of time before trying again)
- To continue on failure, use
  - Catch: ErrorEquals
  - Catch: Next
- Include data in error messages to help with troubleshooting.


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
