# an introduction to observability

Observability lets us understand a system from the outside, by letting us ask questions about that system without knowing its inner workings. Furthermore, it allows us to easily troubleshoot and handle novel problems (i.e. “unknown unknowns”), and helps us answer the question, “Why is this happening?”

Observability has made it possible for both developers and operators to gain that visibility into their systems.

### Telemetry
Telemetry refers to data emitted from a system, about its behavior. The data can come in the form of Traces, Metrics, and Logs.

### Reliability
Reliability answers the question: “Is the service doing what users expect it to be doing?” A system could be up 100% of the time, but if, when a user clicks “Add to Cart” to add a black pair of pants to their shopping cart, and instead, the system keeps adding a red pair of pants, then the system would be said to be unreliable.

### Metrics
Metrics are aggregations over a period of time of numeric data about your infrastructure or application. Examples include: system error rate, CPU utilization, request rate for a given service.

### Service Level Indicator
SLI, or Service Level Indicator, represents a measurement of a service’s behavior. A good SLI measures your service from the perspective of your users. An example SLI can be the speed at which a web page loads.

### Service Level Objective
SLO, or Service Level Objective, is the means by which reliability is communicated to an organization/other teams. This is accomplished by attaching one or more SLIs to business value.

### Spans

A Span represents a unit of work or operation. It tracks specific operations that a request makes, painting a picture of what happened during the time in which that operation was executed.

A Span contains name, time-related data, structured log messages, and other metadata (i.e. Attributes) to provide information about the operation it tracks.

Below is a sample of the type of information that would be present in a Span.

#### Span Attributes

| Key              | Value                                                        |
| ---------------- | ------------------------------------------------------------ |
| net.transport    | IP.TCP                                                       |
| net.peer.ip      | 10.244.0.1                                                   |
| net.peer.port    | 10243                                                        |
| net.host.name    | localhost                                                    |
| http.method      | GET                                                          |
| http.target      | /cart                                                        |
| http.server_name | frontend                                                     |
| http.route       | /cart                                                        |
| http.scheme      | http                                                         |
| http.host        | localhost                                                    |
| http.flavor      | 1.1                                                          |
| http.status_code | 200                                                          |
| http.user_agent  | Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0 Safari/537.36 |