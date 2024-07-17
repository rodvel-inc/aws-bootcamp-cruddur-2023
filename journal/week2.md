# Week 2 — Distributed Tracing

For software, there is debugging. For everything else, there is `Observability`.

We have logs, log streams, but monitoring and observability go beyond that.

We give software instuctions to tell us what is going on under the hood.

**`Distributed tracing`** is the standard in **`Modern observability`**, but it is not about different logs from different components in a non-contextual or non-related structure. 

**`Modern observability`** is about telling a story. The so-called story is better known as the **`trace`**. 

For instance, as a request starts from the backend, it goes through different systems, applications, services, or components. The "marks" left on these elements is what the trace is made up of.

### Honeycomb

COnfiguration
Using OTEL (open telemetry) env vars to send data into the Honeycomb workspace I created

First, install the opentelemetry-api, using Python:

pip install openetelemetry-api

Then, copy in the requirements.txt file the packages that need dowloading:
opentelemetry-api 
opentelemetry-sdk 
opentelemetry-exporter-otlp-proto-http 
opentelemetry-instrumentation-flask 
opentelemetry-instrumentation-requests

Then, inside the backend-flask folder, run:
pip install -r requirements.txt

I am also adding some code in app.py, in order to intialize the opentelemetry API:

from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor

There are other instructions to be added to the app.py file.

So far, I have tested that the commands that will be included in the docker-compose.yml file work as expected.



