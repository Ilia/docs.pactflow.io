---
title: OAS Keyword Support
sidebar_label: OAS Keyword Support
---

# Keyword Support for OAS Contracts

OAS contracts may contain logical keywords `anyOf`, `allOf`, and `oneOf`, which are used to create a complex schema, or validate a value against multiple criteria. In Bi-Directional Contract Testing these keywords are currently partially supported.

## allOf

Because Pactflow sets `additionalProperties` to `false` on response bodies, `allOf` cannot be used to validate a response body against multiple schemas.
See this [write up](https://bitbucket.org/atlassian/swagger-mock-validator/src/master/FAQ.md) on this specific issue. 

## oneOf, anyOf
There is limited support for the `anyOf` and `oneOf` keywords. To compare the anyOf or oneOf schema with a response body from a pact contract you must:

- Ensure the `type` of the bodies match
- If the type of the body is `object` the object's fields need to be marked as `required`. Fields not marked as required will not be considered in the comparison and will not cause the integration to fail. It is recommended to mark all fields that the consumer will use as `required` in the OAS schema used for Bi-Directional Contract Testing.
- It is recommended to programmatically dereference and inline `$refs` in the OAS document uploaded to Pactflow, as they can cause issues when verifying `nullable` fields and nested `$refs` can not be accurately compared with a pact file. This can be accomplished using packages such as [json-schema-merge-allof](https://www.npmjs.com/package/json-schema-merge-allof) and [json-schema-resolve-allof](https://www.npmjs.com/package/json-schema-resolve-allof) (works for anyOf and oneOf as well.)


### Example anyOf schema
```
 "schema": {
    "anyOf": [
        {
            "type": "object",         // verified
            "properties": {
                "age": {
                    "type": "integer"
                },
                "nickname": {         // not verified
                    "type": "string"
                }
            },
            "required": [
                "age"                 // verified
            ]
        },
        {
            "type": "object",
            "properties": {
                "pet_type": {
                    "type": "string",
                    "enum": [
                        "Cat",
                        "Dog"
                    ]
                },
                "hunts": {            // not verified
                    "type": "boolean"
                }
            },
            "required": [
                "pet_type"          // verified
            ]
        }
    ]
}
```

### Example oneOf schema

```
  "schema": {
    "oneOf": [
      {
        "type": "object",           // verified
        "properties": {             // not verified
          "hunts": {
            "type": "boolean"
          },
          "age": {
            "type": "integer"
          }
        }
      },
      {
        "type": "object",            // verified
        "properties": {             // not verified
          "bark": {
            "type": "boolean"
          },
          "breed": {
            "type": "string",
            "enum": [
              "Dingo",
              "Husky",
              "Retriever",
              "Shepherd"
            ]
          }
        }
      }
    ]
  }
```