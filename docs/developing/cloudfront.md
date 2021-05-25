# CloudFront

## Overview

- Content Delivery Network (CDN).
- Improves read performance, content cached at the edge.
- 216 edge locations globally.
- DDos protection
- Integration with Shield, Web Application Firewall.
- Can expose external HTTPS and talk to internal HTTPS backends.

## Origins

### S3 Bucket

- Distribute files globally & cache them at the edge.
- Better security via CloudFront Origin Access Identity (OAI).
- Used as an ingress to upload S3 files.
- Uses OAI + S3 Bucket policy. The OAI User ID is what gets inserted into the bucket policy.
- Can restrict bucket access to force users to go through CloudFront.


### Custom (HTTP

- ALB (must be public, and SG must allow public IP of edge locations)
- EC2 instance (must be public, and SG must allow public IP of edge locations)
- S3 Website
- Any HTTP backend

## High-level Flow

Client => HTTP GET /foo.jpg => Edge Location => S3 Bucket/HTTP (if not cached) => Edge Location (add/update cache) => Client

## Geo Restriction

- Restrict who can access the distribution based on country.
- Uses third party Geo-IP database.

## CloudFront vs S3 CRR

- CloudFront uses a global edge network and files are cached for a TTL (default is 24hrs). Useful for static content.
- S3 CRR needs to be setup in each region replication needs to happen.  Files are uploaded in near realtime, it's read-only and better suited for dynamic content that needs low latency.

## Caching

- Cache based on headers, session cookies, query string parameters.
- Cache sits on each Edge Location.
- Cache TTL can be from 0 seconds to 1 year. Controlled via Cache-Control or Expires header.
- CreateInvalidation API can be used to invalidate part of the cache.
- Maximise cache hits by separating static and dynamic content distributions (static to S3, dynamic to ALB + EC2).

## Security

### Viewer Protocol Policy (client to edge location)

- Redirect HTTP to HTTPS, or Use HTTPS only
- S3 Websites don't support HTTPS

### Origin Protocol Policy (edge location to backend)

- HTTPS only or, match viewer (http => http, https => https)

## Signed URL/Cookies

- Allows visibility and control over which users are allowed to access content.
- Signed URL/Cookie has a policy that:
  - Include URL expiration (minutes to years)
  - Inclue IP ranges to access the data from
  - Trusted signers (which AWS accounts can create signed URLs)
- Signed URL gives access to a specific file.
- Signed Cookie gives access to many files.
- Private key is used by applications to sign the URLs.
- Public key is used by CloudFront to verify the URLs.

### Trusted Key Group

- The recommended way to sign.
- Assigned to a CloudFront distribution.
- Contains one or more public keys so CloudFront can verify the URLs.
- Can use APIs to manage/rotate the keys.

### CloudFront Key Pair

- An AWS account that has the CloudFront Key Pair can also be used for signing.
- Avoid because the root account is needed to manage the keys, and there's no API available.

## CloudFront Signed URL vs S3 Pre-signed URL

### CloudFront Signed URL

- Signed URL allows access to a path, regardless of origin.
- Only the root can manage the account wide key-pair used for signing.
- Can filter by IP, path, date or expiration.
- Can leverage caching features.

### S3 Pre-Signed URL

- Issue a request as the person who pre-signed the URL.
- Uses the IAM key of the signing IAM principal.
- Limited lifetime.

## Price Classes

| Class           | Description                                                |
|-----------------|------------------------------------------------------------|
| Price Class All | Best performance, most expensive, uses all edge locations. |
| Price Class 200 | Excludes the most expensive regions.                       |
| Price Class 100 | Only uses the cheapest regions.                            |

## Multiple Origin Routes

- Route to different origins based on the content type. For example, API calls go to the ALB, static content goes to an S3 bucket.

## Origin Groups

- Increases HA and ability to do failover.
- Has one primary, and one secondary origin.
- Can use S3 buckets to enable region-level H/A (retrieve from secondary bucket in another region if the first bucket returns an error).

## Field Level Encryption

- Protects sensitive user information.
- Information is encrypted at the edge close to the user.
- Up to 10 fields in the request can be encrypted.
- 