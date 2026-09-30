# Rust & Cloud Computing: A Learning Journey

This repo is an index of the Rust and cloud projects I built during my MS in Data Science at Duke. The code lives in separate repos, linked below. This page explains what each one taught me, in the order I'd suggest reading them.


> *These are learning projects, not production systems. Many deliberately use a tiny task (reversing a name, computing a factorial) so the focus stays on the *tooling* rather than the business logic.*

---

## The Big Picture

Each project adds one new idea on top of the last:

```
Rust basics  →  Web service  →  Container  →  CI/CD  →  Serverless  →  Orchestration  →  Data + ML
   (CLI)         (REST API)     (Docker)    (pipelines)   (Lambda)    (Step Functions)   (ClickHouse, Transformers)
```

---

## Stage 1: Learn the language

### [rust_command_line_tool](https://github.com/DivyaSharma0795/rust_command_line_tool)
**Concept:** Ingest, process, output, and test.

A command-line tool that reads student names and marks from a CSV, processes them, and prints a report. It is split into an `ingest_data` function and a `process_data` function, each covered by unit tests.

**What I learned:**
- Rust's `struct`s and how ownership affects passing data between functions
- Why splitting logic into small, testable functions makes code easier to trust
- Writing unit tests with `cargo test`

---

## Stage 2: Build a web service

### [rust_microservice](https://github.com/DivyaSharma0795/rust_microservice)
**Concept:** A REST API, a container, and an automated pipeline.

A small web service built with Warp. Hit `/hello/{name}` and it returns the name reversed. It is packaged with a Dockerfile and has a GitLab CI pipeline.

**What I learned:**
- What a REST endpoint is and how a route maps to a function
- Containerizing a service so it runs the same way everywhere
- **CI/CD:** every push automatically builds and checks the code, so problems surface early

### [rust_actix_web_service](https://github.com/DivyaSharma0795/rust_actix_web_service)
**Concept:** Same idea, different framework.

A web service built with Actix that calculates a factorial from a number in the URL, wrapped in a Docker container.

**What I learned:**
- Comparing Warp and Actix showed me the pattern (route → handler → response) is the same across frameworks
- How a Dockerfile describes a build, step by step

---

## Stage 3: Go serverless

**The idea:** With a traditional service, you rent a server that sits waiting for requests. With AWS Lambda, you upload a function and AWS runs it only when something calls it, so you pay for usage rather than idle time.

### [rust_aws_lambda_function](https://github.com/DivyaSharma0795/rust_aws_lambda_function)
**Concept:** The simplest possible Lambda.

A Rust function that takes a name and returns it reversed, deployed with the AWS CLI.

**What I learned:** The basic lifecycle: write, compile for Lambda, deploy, invoke.

### [rust_lambda_data_processor](https://github.com/DivyaSharma0795/rust_lambda_data_processor)
**Concept:** A Lambda that talks to other AWS services.

Built with Cargo Lambda. It receives a JSON payload, stores the name in a DynamoDB table, and returns the reversed name (`divya` → `ayvid`). It can be triggered through API Gateway, with CloudWatch logs and X-Ray tracing enabled.

**What I learned:**
- **API Gateway** gives a Lambda a public URL
- **DynamoDB** is where the function keeps its data
- **CloudWatch and X-Ray** are how you see what happened when something goes wrong, which matters as much as the code itself

### [rust_serverless_microservice](https://github.com/DivyaSharma0795/rust_serverless_microservice)
**Concept:** Putting the serverless pieces together as a microservice.

Setup notes and code for a serverless Rust microservice on AWS Lambda.

---

## Stage 4: Orchestrate a pipeline

### [rust_lambda_step_function](https://github.com/DivyaSharma0795/rust_lambda_step_function)
**Concept:** One function is a tool; several coordinated functions are a pipeline.

A Rust Lambda plus an AWS Step Functions state machine that coordinates a data processing pipeline. Infrastructure is defined in an AWS SAM template (`template.yaml`), and the workflow definition lives in `pipeline/`.

**What I learned:**
- **Step Functions** let you chain steps, pass data between them, and handle failures in one place
- **Infrastructure as code:** describing cloud resources in a file means they can be reviewed, versioned, and recreated

---

## Stage 5: Data and models

### [rust_vector_database](https://github.com/DivyaSharma0795/rust_vector_database)
**Concept:** Getting data into a database and back out as something you can look at.

A Rust program that generates random values, loads them into a ClickHouse database, queries them with an aggregation, and uses the Plotters library to draw a histogram saved as a PNG.

**What I learned:**
- The full loop: ingest → query → aggregate → visualize
- ClickHouse is built for fast analytical queries over large tables, which is very close to the kind of data I work with in marketing analytics

### [rust_serverless_transformer_endpoint](https://github.com/DivyaSharma0795/rust_serverless_transformer_endpoint)
**Concept:** Serving a machine learning model from a serverless function.

Dockerizes a Hugging Face transformer written for Rust, deploys the container to AWS Lambda, and exposes a query endpoint.

**What I learned:**
- Lambda can run a container image, which lets you bundle a model along with its dependencies
- The trade-offs of serving a model this way, such as cold starts and package size

---

## Reading Guide

| If you want to see... | Start here |
|---|---|
| Rust fundamentals and testing | `rust_command_line_tool` |
| Docker and CI/CD | `rust_microservice` |
| Serverless with real AWS services | `rust_lambda_data_processor` |
| Pipeline orchestration | `rust_lambda_step_function` |
| ML model deployment | `rust_serverless_transformer_endpoint` |

## Tools Used Across These Projects

Rust · Cargo · Warp · Actix · Docker · GitLab CI · AWS Lambda · API Gateway · DynamoDB · Step Functions · AWS SAM · CloudWatch · X-Ray · ClickHouse · Plotters · Hugging Face

---
