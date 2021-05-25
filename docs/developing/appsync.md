# AppSync

## Overview

- Managed service using GraphQL.
- GraphQL makes it easy to perform data queries.
- Can combine data from multiple sources: NoSQL, RDS, HTTP  APIs.
- Integrates with DynamoDB, Aurora, Elasticsearch, Lambda etc.
- Retrieve real-time data with WebSockets or MQTT on WebSocket (AppSync, Application Load Balancer, API Gateway).
- Local data access and data sync for mobile apps (offline sync).
- Uses resolvers to communicate with data sources.
- Integrated with CloudWatch metrics & logs.
- Use CloudFront in front of AppSync to support HTTPS or use a custom domain.

## GraphQL

- GraphQL schema defines the data structure (objects).
- Response is in JSON.

## Security

There's four ways to authorize applications:

- API_KEY
- AWS_IAM: IAM users/roles/cross-account access.
- OPENID_CONNECT: OpenID Connect provider/JSON web token.
- AMAZONG_COGNITO_USER_POOLS.
