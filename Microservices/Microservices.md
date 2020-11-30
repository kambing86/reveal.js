# Microservices Traps and Anit-patterns

## Prepared by Chua Kang Ming

---

## What is Microservices?

- Agile -> Just split your team/code as small as possible and work in parallel<!-- .element: class="fragment" -->

![Image](./Microservices/tenor.gif)<!-- .element: class="fragment" -->

Note: is it something related to the another buzzword from Amazon as well? Agile.

--

## Microservices

- also known as the microservice architecture - is an architectural style that structures an application as a collection of services that are<!-- .element: class="fragment" -->
- Highly maintainable and testable<!-- .element: class="fragment" -->
- Loosely coupled<!-- .element: class="fragment" -->
- Independently deployable<!-- .element: class="fragment" -->
- Organized around business capabilities<!-- .element: class="fragment" -->
- Owned by a small team<!-- .element: class="fragment" -->

Note: Not the same as SOA (Service-Oriented Architecture), to put it simply, service-oriented architecture (SOA) has an enterprise scope, while the microservices architecture has an application scope.

---

## Traps

### Microservices are a magic pixie dust

- believing that a sprinkle of microservices will solve all of your development problems<!-- .element: class="fragment" -->

Note: not addressing development and deployment process, some still doing manual testing by seperate QA team, manual deployment

--

## Traps

### Microservices as the goal

- making the adoption of microservices the goal and measuring success in terms of the number of services written<!-- .element: class="fragment" -->

Note: not addressing code quality

--

## Traps

### Scattershot adoption

- multiple application development teams attempt to adopt the microservice architecture without any coordination<!-- .element: class="fragment" -->

Note: duplicated efforts, common practice should be having a devops team to coordinate the deployment

--

## Traps

### Trying to fly before you can walk

- attempting to adopt the microservice architecture (an advanced technique) without (or not committing to) practicing basic software development techniques, such as clean code, good design, and automated testing<!-- .element: class="fragment" -->

Note: clean code, automated testing, well designed API

--

## Traps

### Focussing on Technology

- focussing on technology aspects of microservices, most commonly the deployment infrastructure, and neglecting key issues, such as service decomposition<!-- .element: class="fragment" -->
- Docker, Kubernetes, Istio and Serverless<!-- .element: class="fragment" -->

--

## Traps

### More the merrier

- intentionally creating a very fine-grained microservice architecture<!-- .element: class="fragment" -->

Note: Each additional service makes development, testing and deployment more complicated, more overhead, start with one service per team, define more services only when it solves a problem

--

## Traps

### Red flag act

- taken from the 19th century Ref flag act that were passed in the US and UK<!-- .element: class="fragment" -->
- required a pedestrian to walk in front of each automobile waving a red flag to warn the drivers and riders of horses<!-- .element: class="fragment" -->
- the automobile is forced to travel at the same speed as a pedestrian<!-- .element: class="fragment" -->
- retaining the same development process and organization structure that were used when developing monolithic applications<!-- .element: class="fragment" -->

Note: organization retains the same process and structure that were used when developing monolithic applications, should change everything (incrementally)

---

## Anti-patterns

### Distribution is free

- Developers connect services as if they are calling another function synchronously<!-- .element: class="fragment" -->
- IPC (REST API, gRPC) is slow and unreliable<!-- .element: class="fragment" -->
- Synchronous communication => temporal coupling => reduced availability<!-- .element: class="fragment" -->

---

### Example of temporal coupling

<div>

![Image](./Microservices/temporal_coupling.png)<!-- .element: class="fragment" -->

- availability = availability(User Service) x availability(Payment Service)<!-- .element: class="fragment" -->
- 0.81 = 0.90 x 0.90<!-- .element: class="fragment" -->

</div>

--

### Example of temporal coupling

- Imagine if you have 50 microservices with 99% of availability<!-- .element: class="fragment" -->
- 0.99 ^ 50 ≈ 0.605<!-- .element: class="fragment" -->
- if you have one interaction that only use 10 microservices, the availability is about 0.904<!-- .element: class="fragment" -->

--

## Solution

### Intermediate State with Event Bus

- Have a Pending Status for complex interaction<!-- .element: class="fragment" -->
- Message Queue / Broker, eg. RabbitMQ<!-- .element: class="fragment" -->
- Event Streaming, eg. Apache Kafka<!-- .element: class="fragment" -->

--

## Decentralised

![Image](./Microservices/decentrialized.png)<!-- .element: style="background: #fff;" -->

---

## Anti-patterns

### Shared Database

- A database is shared by multiple services<!-- .element: class="fragment" -->
- A change to the table affects multiple services<!-- .element: class="fragment" -->
- Service isolation is impossible<!-- .element: class="fragment" -->
- Example: User service accesses to Payment Table<!-- .element: class="fragment" -->

--

## Solution

### Database per service

<ul>
  <li>Other services <b>MUST ONLY</b> able to access the data through API</li><!-- .element: class="fragment" -->
</ul>

---

## Anti-patterns

### Unencapsulated service

- Service expose CRUD operation<!-- .element: class="fragment" -->
- A change of schema => API breaking change<!-- .element: class="fragment" -->

--

## Solution

### Encapsulation

- Hide as much implementation details as possible<!-- .element: class="fragment" -->
- Don't use any codegen tools to convert ORM code to API<!-- .element: class="fragment" -->

---

## Anti-patterns

### Distributed Monolith

- Sharing codes in between services<!-- .element: class="fragment" -->
- Update to single line of code affects multiple services<!-- .element: class="fragment" -->
- Unclear ownership of the shared codes<!-- .element: class="fragment" -->

--

## Solution

### Form a new team for shared code

- Facebook forms a core team to maintain React<!-- .element: class="fragment" -->
- Google - Angular team<!-- .element: class="fragment" -->

---

## References

<ul>
  <li><a href="https://microservices.io/microservices/antipatterns/-/the/series/2019/06/18/microservices-adoption-antipatterns.html">Link 1</a></li>
  <li><a href="http://chrisrichardson.net/post/antipatterns/2019/01/28/melbourne-microservices.html">Link 2</a></li>
</ul>
