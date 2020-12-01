# Microservices Anti-patterns

## Prepared by Chua Kang Ming

---

## What is Microservices?

- Agile -> Just split your team/code as small as possible and work in parallel<!-- .element: class="fragment" -->

![Image](./Microservices/tenor.gif)<!-- .element: class="fragment" -->

Note: is it related to the another buzzword from Amazon as well? Agile.

--

## Bare Minimum Microservice

![Image](./Microservices/bare_minimum.png)<!-- .element: class="fragment" -->

Note: no kubernetes back in 2015, just have EC2 in AWS and deploy, then we have ECS(2015) and EKS(2018) in AWS lately

--

## Modern approach

<div class="r-stack">

![Image](./Microservices/api_gateway.png)<!-- .element: class="fragment fade-in-then-out" -->

![Image](./Microservices/monolith-frontback-microservices.png)<!-- .element: class="fragment fade-in-then-out" -->

![Image](./Microservices/verticals-headline.png)<!-- .element: class="fragment fade-in-then-out" -->

</div>

Note: BFF(Backend for Frontend)/API Gateway might be doing load balancing, service discovery integration, logging, tracing, authentication and authorization

--

## Microservices

- also known as the microservice architecture - is an architectural style that structures an application as a collection of services that are<!-- .element: class="fragment" -->
- Highly maintainable and testable<!-- .element: class="fragment" -->
- Loosely coupled<!-- .element: class="fragment" -->
- Independently deployable<!-- .element: class="fragment" -->
- Organized around business capabilities<!-- .element: class="fragment" -->
- Owned by a small team<!-- .element: class="fragment" -->
- Main purpose is to make the application/team scalable<!-- .element: class="fragment" -->

Note: Not the same as SOA (Service-Oriented Architecture), to put it simply, service-oriented architecture (SOA) has an enterprise scope, while the microservices architecture has an application scope.

--

## Cloud Native

![Image](./Microservices/cloud-native-diagram.png)<!-- .element: class="fragment fade-in-then-out" -->

Note: Cloud-native app development typically includes devops, agile methodology, microservices, cloud platforms, containers like Kubernetes and Docker, and continuous delivery—in short, every new and modern method of application deployment.

---

## Anti-patterns

---

## Non-technical

### Microservices are a magic pixie dust

- believing that a sprinkle of microservices will solve all of your development problems<!-- .element: class="fragment" -->

Note: not addressing development and deployment process, some still doing manual testing by seperate QA team, manual deployment

--

## Non-technical

### Microservices as the goal

- making the adoption of microservices the goal and measuring success in terms of the number of services written<!-- .element: class="fragment" -->

Note: normally measuring the success in terms of the lead time (how soon the team can deploy a fix to production)

--

## Non-technical

### Scattershot adoption

<ul>
  <li>multiple application development teams attempt to adopt the microservice architecture without any coordination</li><!-- .element: class="fragment" -->
  <li><a href="https://newsignature.com/articles/thinking-microservices-need-devops-first/">Thinking about Microservices? You need DevOps first</a></li><!-- .element: class="fragment" -->
</ul>

Note: duplicated efforts, common practice should be having a devops team to coordinate the deployment

--

## Non-technical

### Trying to fly before you can walk

<div class="r-stack">

![Image](./Microservices/microservice_prerequisites.png)<!-- .element: class="fragment fade-in-then-out" -->

attempting to adopt the microservice architecture (an advanced technique) without (or not committing to) practicing basic software development techniques, such as clean code, good design, and automated testing<!-- .element: class="fragment" -->

</div>

Note: using microservices is going to have a cost, and with bad code quality, microservices get more pain than gain

--

## Non-technical

### Focussing on Technology

- focussing on technology aspects of microservices, most commonly the deployment infrastructure, and neglecting key issues, such as service decomposition<!-- .element: class="fragment" -->
- Docker, Openshift, Kubernetes, Istio and Serverless<!-- .element: class="fragment" -->

--

## Non-technical

### More the merrier

- intentionally creating a very fine-grained microservice architecture<!-- .element: class="fragment" -->

Note: Each additional service makes development, testing and deployment more complicated, more overhead to deal with, start with one service per team, define more services only when it solves a problem

--

## Non-technical

### Red flag act

<div class="r-stack">

![Image](./Microservices/red_flag.jpeg)<!-- .element: class="fragment fade-in-then-out" -->

<div>

- taken from the 19th century Ref flag act that were passed in the US and UK<!-- .element: class="fragment" -->
- required a pedestrian to walk in front of each automobile waving a red flag to warn the drivers and riders of horses<!-- .element: class="fragment" -->
- the automobile is forced to travel at the same speed as a pedestrian<!-- .element: class="fragment" -->

</div>

--

## Non-technical

### Red flag act

<div class="r-stack">

![Image](./Microservices/bad_team.png)<!-- .element: class="fragment fade-in-then-out" -->

retaining the same development process and organization structure that were used when developing monolithic applications<!-- .element: class="fragment fade-in-then-out" -->

</div>

Note: should change everything (incrementally)

--

### Red flag act

<div class="r-stack">

![Image](./Microservices/good_team.png)<!-- .element: class="fragment fade-in-then-out" -->

<blockquote class="fragment fade-in-then-out">
  <p>
    Any organization that designs a system (defined broadly) will produce a design whose structure is a copy of the organization's communication structure.<br />
    - Melvin Edward Conway (1967)
  </p>
</blockquote>

Note: Conway is perhaps most famous for developing the concept of coroutines. A team should ideally own just one service since that’s sufficient to ensure team autonomy and loose coupling and each additional service adds complexity and overhead. A team should only deploy its code as multiple services if it solves a tangible problem, such as significantly reducing lead time or improving scalability or fault tolerance.

</div>

---

## Technical

### Distribution is free

- Developers connect services as if they are calling another function synchronously<!-- .element: class="fragment" -->
- IPC (REST API, gRPC) is slow and unreliable<!-- .element: class="fragment" -->
- Synchronous communication => temporal coupling => reduced availability<!-- .element: class="fragment" -->

---

### Temporal coupling

<div>

![Image](./Microservices/temporal-coupling.png)<!-- .element: class="fragment" -->

- availability = availability(Order Service) x availability(Customer Service)<!-- .element: class="fragment" -->
- 0.81 = 0.90 x 0.90<!-- .element: class="fragment" -->

</div>

--

### Example of temporal coupling

- Imagine if you have 20 microservices with 99% of availability<!-- .element: class="fragment" -->
- 0.99 ^ 20 ≈ 0.82<!-- .element: class="fragment" -->
- your browser might get timeout from server, due to the total time of the whole complex interaction exceed 60 seconds.<!-- .element: class="fragment" -->

--

## Solution

### Reduce tight coupling

- Carefully consider how services interact<!-- .element: class="fragment" -->
- Intermediate Status with asynchronous communication<!-- .element: class="fragment" -->
- Message Queue / Broker, eg. RabbitMQ<!-- .element: class="fragment" -->
- Event Streaming, eg. Apache Kafka<!-- .element: class="fragment" -->

Note: respond immediately without doing the complex processing

--

### Sync vs Async Communication across Microservices

![Image](./Microservices/sync-vs-async-patterns-across-microservices.png)

--

### Decentralised (Event-driven Architecture/Saga)

![Image](./Microservices/decentrialized.png)<!-- .element: style="background: #fff;" -->

Note: 2 Phase Commit(2PC) to Saga

---

## Technical

### Shared Database

- A database is shared by multiple services<!-- .element: class="fragment" -->
- A change to the table affects multiple services, require coordination across teams<!-- .element: class="fragment" -->
- Service isolation is impossible<!-- .element: class="fragment" -->

Note: usually caused by splitting one microservice to 2, or transition from monolith to microservices, (no failure isolation, deadlock might happened)

--

## Solution

### Database per service

<ul>
  <li>Duplicate the DB to the microservice, then dedupe data</li>
  <li>Other services <b>MUST ONLY</b> able to access the data through API</li><!-- .element: class="fragment" -->
</ul>

---

## Technical

### Unencapsulated service

- Service expose CRUD operation<!-- .element: class="fragment" -->
- A change of schema => API breaking change<!-- .element: class="fragment" -->

--

## Solution

### Encapsulation

- Hide as much implementation details as possible<!-- .element: class="fragment" -->
- Don't use any codegen tools to convert ORM code to API<!-- .element: class="fragment" -->

---

## Technical

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
  <li><a href="https://www.redhat.com/en/topics/microservices/what-are-microservices">What are microservices?</a></li>
  <li><a href="https://docs.microsoft.com/en-us/dotnet/architecture/microservices/architect-microservice-container-applications/direct-client-to-microservice-communication-versus-the-api-gateway-pattern">The API gateway pattern versus the Direct client-to-microservice communication</a></li>
  <li><a href="https://micro-frontends.org/">Micro frontends</a></li>
  <li><a href="https://microservices.io/microservices/antipatterns/-/the/series/2019/06/18/microservices-adoption-antipatterns.html">Microservices adoption antipatterns</a></li>
  <li><a href="https://techbeacon.com/app-dev-testing/want-develop-great-microservices-reorganize-your-team">Want to develop great microservices? Reorganize your team</a></li>
  <li><a href="https://microservices.io/patterns/decomposition/service-per-team.html">Service per team</a></li>
  <li><a href="https://docs.microsoft.com/en-us/dotnet/architecture/microservices/architect-microservice-container-applications/communication-in-microservice-architecture">Communication in a microservice architecture</a></li>
  <li><a href="https://dzone.com/articles/event-driven-orchestration-an-effective-microservi">Event-Driven Orchestration</a></li>
</ul>
