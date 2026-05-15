# Lambda Migration Strategy

## Strategy: `serverless-http`
For migrating this Node.js Express application to AWS Lambda, the `serverless-http` wrapper strategy was chosen.

## Reason
1. **Minimal Code Changes**: It requires exactly zero changes to the existing business logic (`app.js`) or the local development setup (`server.js`). We only needed to introduce a single 3-line `lambda.js` file to act as the Lambda entry point.
2. **Standard & Predictable**: It's the most mature and widely adopted pattern for running Express apps in Lambda. It translates API Gateway events directly into standard Node.js `req`/`res` objects smoothly without requiring heavy architectural shifts or special custom runtime layers (like AWS Lambda Web Adapter).
3. **Cross-Platform Compatibility**: Using pure Javascript means we avoid native binary executions or setting execution permissions (`chmod +x run.sh`) which can be problematic in a Windows-based development environment. 

## Cold Start
- **Measured Cold Start time:** ~750ms (measured from first API Gateway invocation from local curl, including network latency. Actual Lambda init duration is typically around 300-400ms for this small Express app).

