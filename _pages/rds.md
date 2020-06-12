---
title: "RDS"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /rds/
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
sidebar:
  nav: "docs"
---

## What is RDS?

- Managed DB service that uses SQL.
- Database name needs to be unique across the region.
- Storage can auto-scale.
- VPC can't be changed once the instance is created.
- Can connect using IAM users and roles.

## Types of RDS

- Postgres
- MySQL
- MariaDB
- Oracle
- MS SQL Service
- Aurora (AWS proprietary database).

## Why RDS instead of DB on EC2?

- Automated provisioning and OS patching.
- Continuous backups & point in time restores.
- Monitoring dashboards.
- Read replicas for improved read performance.
- Multi-AZ for DR.
- Maintenance windows for upgrades.
- Scaling capabilities (horizontal/vertical).
- Storage backed by EBS.
- No SSH access.

## RDS Backups

- Automatically enabled.
- Daily full backups during maintenance windows.
- Transaction logs backed up every 5mins.
- 7 day retention of automated backups (can be increased to 35 days).
- Manually triggered DB snapshots.
- Snapshots retained as long as needed.

## RDS Read Replicas

- Help to scale read performance/capacity.
- Create upto 5 read replicas within the same AZ, cross AZ, or cross region.
- Asynchronous replication.
- A read replica can be promoted to it's own database (effectively cloning the db server).
- Applications must update the connection string to leverage read replicas.
- Use case: Run heavy reporting & analytics against an application without impacting production performance.
- There's a netwok cost transferring data between AZs. Can save money by keeping read replicas in the same AZ.

## RDS Multi-AZ

- Used for disaster recovery.
- Synchronous replication between 2 RDS in different AZs.
- One DNS name, two hosts.
- Automatic failover to stand-by database which increases availability.
  - Doesn't require any intervention in the applications.
- Not used for scaling.
- Read replicas can be setup as Multi AZ for DR.

## RDS Encryption

- Master and read replicas can be encrypted using AWS KMS (AES-256).
- Encryption must be defined at launch time.
- Read replicas can't be encrypted if the master isn't.
- Enable transparent data encryption (TDE) for Oracle and SQL Server.
- SSL certs to encrypt data in transit.
  - Need to configure the database to ensure SSL.
  - Postsql is a parameter group to enable encryption.
- Snapshots of un-encrypted databases are also un-encrypted.
- Snapshots of an encrypted database are also encrypted.
- Can copy a snapshot into an encrypted one.
- TDE not supported by Postgresql.

### Encrypting an un-encrypted database

- Take snapshot of the un-encrypted database.
- Copy the snapshot, and enable encryption.
- Restore the database from the encrypted snapshot.
- Migrate applications to the new database, and delete the old one.

## Network Security

- Deploy to private subnet.
- Uses security groups that are attached to the RDS instance.
- Controls IP/security groups that can communicate with RDS.

## IAM Security

- IAM policies help control who can manage the RDS.
- Traditional username/password to login to the database.
- IAM-based authentication for MySQL and PostgreSQL. Not supported by Oracle.

### IAM Authentication

- Get auth token from IAM and RDS API calls.
- Auth token has a 15min lifetime.
- EC2 instance is given an IAM role to allow it to request an auth token from the RDS service via API.
- EC2 instance uses SSL encryption and passes the auth token through to the RDS instance.

#### Benefits of IAM authentication

- Requires SSL.
- IAM roles and EC2 instance profiles for easy migration.

## RDS Shared Responsibility

### Users

- Check ports/IP/security group inbound rules.
- In-database user creation and permissions, or manage via IAM.
- Create database with/without public access.
- Ensure parameter groups or DB is configured to only allow SSL connections.

### AWS

- No SSH access.
- No manual DB patching.
- No manual OS patching.
- No way to audit the underlying instance.
