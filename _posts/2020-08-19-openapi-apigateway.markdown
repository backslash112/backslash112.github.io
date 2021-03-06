---
layout: post
title:  "How to use OpenAPI 3.0 with AWS API Gateway to define your APIs"
date:  2020-08-19
description: ""
img: aws.png
tags: [aws, api, openapi3, swagger]
---

## The basic usage of OpenAPI 3.0 in AWS SAM

```
# sam.yaml
Resources:
  PrivateApi:
    Type: 'AWS::Serverless::Api'
    Properties:
      Name: !Sub ${service}-${branch}-private-api
      StageName: dev
      ...
      DefinitionBody: !Sub |
        openapi: '3.0.0'
        ...
        info:
          title: ${service}
          version: "1.0"
        basePath: "/dev"
        paths:
          /users/{userId}:
            get:
              ...
```

## Add request parameters/body

### Request parameters defination

```yaml
/user
  get:
    description: Query user by user ID
    parameters:
      - in: path
        name: userId
        required: true
        type: string
```

### Request body defination

```yaml
/user
  post:
    requestBody:
      required: true
      content:
        application/json:
          schema:
            type: array
            minItems: 1
            maxItems: 100
            items:
              type: object
              properties:
                userId:
                  type: string
                userName:
                  type: string
                enable:
                  type: boolean
              required:
                - userId
                - userName
                - enable
```

## Enable validation in AWS API Gateway

```yaml
path:
  ...
x-amazon-apigateway-request-validators:
    all:
      validateRequestParameters: true
      validateRequestBody: true
```

## Resources

1. https://github.com/awslabs/serverless-application-model/issues/1250
2. https://docs.aws.amazon.com/lambda/latest/dg/services-apigateway-template.html (Another way to setup api using lambda events.)
3. https://github.com/awslabs/serverless-application-model/issues/579 (AWS::Serverless::Api - MethodSettings)
4. https://swagger.io/docs/specification/data-models/data-types/#range
5. https://swagger.io/docs/specification/describing-request-body/
6. https://swagger.io/specification/
