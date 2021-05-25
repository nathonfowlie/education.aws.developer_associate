# AWS Certificate Manager (ACM)

## Overview

- Host public SSL certificates in AWS.
  - BYO and upload using the CLI or
  - Have ACM provision and renew the SSL certs (free).
- Can manage private certificates as well.

## Integration

Can load certs onto:

- Load Balancers (including ones created by Elastic Beanstalk).
- Cloudfront distributions.
- APIs on API gateways.

