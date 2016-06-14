---
layout: post
title: Amazon AWS Quick Tips - Delete all AWS Cognito identities
date: 2016-06-14 11:44
author: Peter
comments: true
categories: [AWS Cognito, AWS-CLI, JQ]
---
AWS Console does not provide a way to **delete all of the identities in your identity pool**. AWS CLI does.

### Install jq

If ```jq``` is not installed on your machine, follow the instructions on [jq website](https://stedolan.github.io/jq/).

### Know your identity pool id

Find the identity pool id you want to work with. You can use the AWS Console or the following command, by replacing the YOUR_REGION code to the desired region code:

```sh
aws cognito-identity list-identity-pools --max-results 60 --region [YOUR_REGION]
```

### Listing the identites
Armed with the identity pool id, use the following command to list the first 60 identities in your identity pool, coma separated:

```sh
aws cognito-identity list-identities --region [YOUR_REGION] --identity-pool-id [YOUR_IDENTITY_POOL_ID] --max-results 60 | jq -r '.Identities | map(.IdentityId) | join("\" \"")' | sed -e 's/\(.*\)/\"\1\"/'
```

### Deleting the identites
We were selecting only 60 identites because this is a limitation on the ```delete-identities``` function. Copy the result and paste it into the following command:

```sh
cognito-identity delete-identities --region [YOUR_REGION] --identity-ids-to-delete
```

### Merging all together
Do you want to delete the next 60 identities with a single command? Run the following: 

```sh
aws cognito-identity list-identities --region [YOUR_REGION] --identity-pool-id [YOUR_IDENTITY_POOL_ID] --max-results 60 | jq -r '.Identities | map(.IdentityId) | join("\" \"")' | sed -e 's/\(.*\)/\"\1\"/' | xargs aws cognito-identity delete-identities --region [YOUR_REGION] --identity-ids-to-delete
```
