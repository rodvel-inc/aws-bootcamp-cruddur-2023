# Week 2 — Distributed Tracing

- For software, there is debugging. For everything else, there is `Observability`.

- We have logs, log streams, but monitoring and observability go beyond that.

- We give software instuctions to tell us what is going on under the hood.

**`Distributed tracing`** is the standard in **`Modern observability`**, but it is not about different logs from different components in a non-contextual or non-related structure. 

**`Modern observability`** is about telling a story. The so-called story is better known as the **`trace`**. 

- For instance, as a request starts from the backend, it goes through different systems, applications, services, or components. 
- The "marks" left on these elements is what the trace is made up of.

### Honeycomb

#### Configuration
- Use `OTEL``(open telemetry) **env vars** to send data into the Honeycomb workspace I created.
- Before anything, export the **env vars** that will be used later on by the `docker-compose.yml` file (or make sure that they are already there):
`
```sh
export HONEYCOMB_API_KEY=""
export HONEYCOMB_SERVICE_NAME="backend-flask"
gp env HONEYCOMB_API_KEY=""
gp env HONEYCOMB_SERVICE_NAME="backend-flask"
```
- The API KEY has to be generated at honeycomb.io

- Now, install the opentelemetry-api, using Python:

```sh
pip install opentelemetry-api
```

- Then, append in the `requirements.txt` file the packages that need dowloading:
  - opentelemetry-api 
  - opentelemetry-sdk 
  - opentelemetry-exporter-otlp-proto-http 
  - opentelemetry-instrumentation-flask 
  - opentelemetry-instrumentation-requests

- Then, inside the `backend-flask` folder, run:

```sh
pip install -r requirements.txt
```

- Also, I am adding some code in `app.py` in order to intialize the opentelemetry API:

```python
from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
```

There are other instructions to be added to the `app.py` file.

So far, I have tested that the commands that will be included in the `docker-compose.yml` file work as expected.

## What is observability in AWS?

- It is a set of open-source solutions that give you the ability to understand what is happening across your technology stack at any time.

- It lets you collect, correlate, aggregate, and analyze telemetry in your network, infrastructure, and applications in the cloud, hybrid or on-prem environments so you can gain insights into the behavior, performace and health of the systems.

### Observability Pillars

- Metrics 
- Traces 
- Logs

### Observability Services

- AWS CloudWatch Metrics
- AWS X Ray Traces
- AWS CloudWatch Logs

## Instrument AWS X-Ray

- It helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture.
- It allows you to understand how your application and its underlying services are performing to identify and troubleshoot the root cause of performance issues and errors.
- It provides an end-to-end view of requests as they travel through your application, and shows a map of your application's underlying components.

### How to use it

- Instrument your application, which allows X-Ray to track how your application processes a request.
    - Use the X-Ray SDKs, APIs, or ClouWatch Application Signals to send trace data to X-Ray.
- Optionally, configure X-Ray to work with other AWS Services that integrate with X-Ray.
- Deploy your instrumented application.
    - As your application receives requests, the SDK will record trace, segment and subsegment data.

### Steps

The X-Ray SDK for Python is a library for Python web applications that provides classes and methods for generating and sending trace data to the X-Ray daemon. 

Trace data includes information about incoming HTTP requests served by the application, and calls that the application makes to downstream services using the AWS SDK, HTTP clients, or an SQL database connector. 

- Install the AWS SDK (for Python, in this case) using the command:
```sh
pip install aws-xray-sdk
```
- An alternative to the instruction before this one, you can simply add the following instruction into the `requirements.txt` file.
```sh
aws-xray-sdk
```

- Intall the SDK's testing dependencies:
```sh
pip install tox
```
As with the SDK, this last instruction can also be added to the `requirements.txt` file.

- If you use Flask, start by adding the SDK **`middleware`** to your application to trace incoming requests. 

- The middleware creates a segment for each traced request, and completes the segment when the response is sent. 

### Adding the middleware to your application (flask)

- As with Honeycomb, you have to add code to the `app.py`.

- To instrument your Flask application, first configure a segment name on the xray_recorder. 

- Then, use the XRayMiddleware function to patch your Flask application in code.







