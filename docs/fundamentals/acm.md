# AWS Certificate Manager (ACM)

## Overview

Service used to host public SSL certificates in AWS.

Private certificates can also be managed, and you can upload existing certificates via the cli.

???+ note Note
    Certificates provisioned via ACM are free.

## Integration

ACM certificates can be loaded onto

- Load Balancers (including ones created by Elastic Beanstalk).
- Cloudfront distributions.
- APIs on API gateways.

