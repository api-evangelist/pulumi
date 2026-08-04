---
title: "Migrate CloudFormation to Pulumi with Discovered Stacks"
url: "https://www.pulumi.com/blog/discovered-stacks-migrate-cloudformation-to-pulumi/"
date: "2026-07-30"
author: "Alejandro Cotroneo"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
With Discovered Stacks , Pulumi Cloud does the bookkeeping for a CloudFormation migration: every resource in the stack gets an explicit migration status, and the migration is done when the code provably matches the cloud. In this tutorial, we take one real CloudFormation stack from discovered to migrated and managed by Pulumi IaC, end to end. What we’re migrating Our example is payments-api , a CloudFormation stack with 61 resources: a VPC, an Aurora ledger database behind an RDS Proxy, an assets S3 bucket, a DynamoDB ledger table, a Kinesis payment-events pipeline, and the IAM roles, KMS keys
