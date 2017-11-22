# GraphQL

## Modern API

---

## Background

- created by Facebook back in 2012
- discussed openly in January 2015, gained a lot of attentions
- redesigned and open sourced by Facebook in July 2015
- implementation relicensed under MIT, specification under Open Web Foundation Agreement (OWFa) in September 2017
- implemented in 10+ programming languages

---

## Who's using it

<div style="display:flex">
  <div style="flex: 1 0 auto;">
    <ul>
      <li>Facebook</li>
      <li>Github</li>
      <li>Coursera</li>
      <li>Pinterest</li>
      <li>Twitter</li>
      <li>Shopify</li>
      <li>
        Scaphold
        <ul>
          <li>Visa</li>
          <li>National Geographic</li>
        </ul>
      </li>
    </ul>
  </div>
  <div style="flex: 1 0 auto;">
    <ul>
      <li>Walmart</li>
      <li>IBM</li>
      <li>New York Times</li>
      <li>Yelp</li>
      <li>Serverless Framework</li>
      <li>Startups.co</li>
      <li>Google</li>
    </ul>
  </div>
</div>

---

## REST vs GraphQL

![Image](./GraphQL/REST vs GraphQL.png)
multiple REST roundtrips vs one GraphQL request

---

## Example

- REST API<br/>https://swapi.co/
- GraphQL API<br/>http://graphql.org/swapi-graphql

---

## RESTful API

![Image](./GraphQL/REST_API.jpg)<!-- .element: style="width: 50%" -->

--

## Issues with REST API

- inefficient: overfetching & underfetching
  - add complexity to join all the api calls
  - REST API - 8 requests, 26.9KB
  - GraphQL API (wrapping REST API) - 1 request, 3.9KB
    - ~7x bandwidth saved, reduce complexity, simple and direct query

--

## Issues with REST API

<div>

- design difficulties
  - Hypermedia links (Included Relationship or Endpoint)
    - /job/{id}

```js
{
  "id": 1,
  "jobPostId": "JOB-123",
  "skills": [
    "/skill/1",
    "/skill/2"
  ]
}

// or

{
  "id": 1,
  "jobPostId": "JOB-123",
  "skills": [
    "Javascript",
    "HTML"
  ]
}
```

<!-- .element: class="line-numbers" -->

</div><!-- .element: style="margin: 0 auto; height: 80vh; overflow-y: auto;" -->

--

## Issues with REST API

- design difficulties
  - nested as store or seperate endpoint
    - /job/{id}/skills
    - /skills/?jobId=[jobId]

--

## Issues with REST API

- inflexible: hard to keep up with rapidly changing requirements of the clients
  - have to adjust the endpoints based on the requirements
  - should our /jobs/search include application link / map location (postal code) or not?
- weakly-typed, hard to share with third party
  - swagger and documentation helps, but couldn't solve all the issues

--

![Image](./GraphQL/api making.jpeg)<!-- .element: style="width: 80%" -->

---

## GraphQL Schema

## Basic building blocks

- Type system (defining Type for query using SDL)
  - similar to swagger yaml definition
- Field resolvers (defining resolve function for Field)
  - similar to route controller in swagger
- They are so similar that there is one tool to convert Swagger to GraphQL with nearly zero afford
  - https://github.com/yarax/swagger-to-graphql

--

## Wrapping/Taming REST API with GraphQL

<div style="position: relative;">
![Image](./GraphQL/wrapped.jpg)<!-- .element: style="width: 80%;" -->
![Image](./GraphQL/cross.png)<!-- .element: class="fragment" style="position: absolute; top: 45%; left: 50%; height: 100%; background: none; transform: translate(-50%, -50%); border: none;" -->
</div>

https://technology.fairfaxmedia.co.nz/taming-rest-apis-using-graphql/

--

![Image](./GraphQL/graphql_rest_layer.png)<!-- .element: style="width: 80%; background: white;" -->
https://rest-layer.io/

---

## Summary

- Similar: Both have the idea of a resource, and can specify IDs for those resources.
- Similar: Both can be fetched via an HTTP GET request with a URL.
- Similar: Both can return JSON data in the request.

--

## Summary

- Different: In REST, the endpoint you call is the identity of that object. In GraphQL, the identity is separate from how you fetch it.
- Different: In REST, the shape and size of the resource is determined by the server. In GraphQL, the server declares what resources are available, and the client asks for what it needs at the time.
- A more efficient Alternative to REST

---

## Bonus: GraphQL with objection.js(ORM)

- objection-graphql
- use the models built for objection.js to build the schema
- require to define the JSON schema and relation mappings
- can be used to query the whole DATABASE!! (useful for rapid prototyping)

---

# Demo

---

## Links

- http://graphql.org/
- https://www.howtographql.com/
- https://dev-blog.apollodata.com/graphql-vs-rest-5d425123e34b
- https://nordicapis.com/7-unique-benefits-of-using-graphql-in-microservices/
- https://medium.freecodecamp.org/give-it-a-rest-use-graphql-for-your-apis-40a2761e6336
- https://marmelab.com/blog/2017/09/04/dive-into-graphql-part-i-what-s-wrong-with-rest.html

---

## Video

<iframe width="640" height="360" src="https://www.youtube.com/embed/a52vko4BcNA" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

---

## Takeaway

- start from new API, such as profile
- not switching all to GraphQL overnight
- by the time the v2 RESTful API is up, GraphQL should be able to use the use the same logic to retrieve the data
