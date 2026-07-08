# 010. Parsing the Request Body from API Gateway HTTP API Events

## Context
SubmitRequestLambda was built and tested successfully using a direct test event in the Lambda console. That test event had requester_id and description sitting at the top level of the event object. When the same function was wired up behind API Gateway and called through the real HTTPS endpoint, every request failed validation with "requester_id and description are required fields," even though the request clearly included both fields.

## Decision
Parse event['body'] as a JSON string before reading any fields out of it, rather than reading fields directly off the top level of event.

## Reasoning
API Gateway HTTP APIs do not pass the request body as flat keys on the event object. The actual body arrives as a JSON encoded string under event['body']. The rest of event carries metadata about the HTTP request itself, like headers, route information, and JWT claims. A Lambda function tested directly in the console with a hand built event can look correct while quietly assuming the wrong shape, since nothing forces that test event to match what API Gateway actually sends.
The fix was one line added before reading requester_id and description:
body = json.loads(event.get('body') or '{}')
Both fields are then read from body instead of from event directly.

## Alternatives Considered
Leaving the code as is and instead reshaping the console test event to match. Rejected because it fixes the test, not the function, and leaves the same failure waiting for the first real call from anywhere other than that one test event.

## Revisit If
A future Lambda function behind this API is built and validated only through console test events before being wired to a real route. The same event shape mismatch will happen again unless body parsing is included from the start.
