---
title: Pactflow On-Premises Architecture
sidebar_label: Architecture
---

## System architecture

### Example AWS architecture

![System architecture](/img/SaaS%20Architecture.png)


### Minimum requirements

* An application server capable of running Docker
* Postgres database
* SAML IDP for SSO

### Recommended architecture

* Deploy to a service designed for managing Docker containers (ECR, Fargate, Kubernetes etc.)

## Internal architecture

The Pactflow On-Premises application is distributed as a Docker image. It is based on the open source [Pact Broker](https://github.com/pact-foundation/pact_broker), which is a Ruby application.

### Application user requirements

The Pactflow application does not need any elevated privileges to run. It runs under the user `app:app`.

### Application port

The Pactflow application runs on port `9292`.

### Healthcheck endpoint

A healthcheck endpoint for use by a Docker container managment service is available at `http://<HOST>/diagnostic/status/elb-heartbeat`. No authentication is required. This endpoint does not make a connection to the database.

To check the connection to the database, use the endpoint `/diagnostic/status/dependencies`. This endpoint should not be used by Docker container managment services, as unrelated database issues might cause the Docker container to churn.