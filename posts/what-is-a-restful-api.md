---
date: "2019-14-15"
title: What is a RESTful API? (notes)
---

_It's in job ads everywhere: employers looking for developers with experience in building RESTful APIs. What does REST mean and what does it take for an API to be considered RESTful?_

**REST is a way to build a web service.** Think of a web service as a common platform that allows communication between client and server applications. So, REST is just a way to do that! It's an architectural style, and it stands for _Representational State Transfer_. [Roy Fielding](http://roy.gbiv.com/), [who gave us REST](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm), explains it like this:

> "REST is intended to evoke an image of how a well-designed web app
> behaves: A network of web pages (a virtual state-machine) where the
> user progresses through an application by selecting links (state
> transitions), resulting in the next page (representing the next
> state of the application) being transferred to the user and rendered
> for their use."

Here is a real-life example:

1. User opens browser (the browser is the HTTP client)
2. User points the browser to a URI (unique resource identifier)
   - the URI identifies where the resource lives
3. The browser sends an HTTP request to that URI
4. The server on the other end does some magic, then sends an HTTP response message back to the browser
   - This response message contains a **representation** of the resource the browser has requested
5. The browser interprets the representation and displays it
   - In other words, our browser (the HTTP client, remember) has changed **state**
6. The client changes _**state**_ depending on _**representation**_ of the resource we're accessing.

The HTTP client can be a browser, but often it's an application.

**REST is defined by six constraints** (constraints are just design decisions):

1. Client-server constraint
   - Client and server are separated
   - Client shouldn't be concerned with how data is stored or how representation is generated
   - Server shouldn't be concerned with user interface, or user state, or anything related to how client is implemented
   - What's the why?: Client and server can thus evolve separately
2. Statelessness constraint
   - The state necessary to handle any request is contained within the request itself
   - Requests cannot take advantage of any stored context on the server; session state is kept entirely on the client, via caching
   - What's the why?: Visibility, reliability, scalability
3. Cacheable constraint
   - Each response must explicitly state if it can be cached or not
   - If response is cacheable, then client cache is given right to reuse that response data for later, equivalent requests
4. Layered system constraint
   - Restricts knowledge to a single layer
   - Solution can be comprised of multiple layers
   - Client cannot tell what layer it's connected to, or if it's connected to an intermediary layer along the way
5. Code on demand constraint (optional)
   - Server can extend or customize client functionality
   - Example: if client is a web app, server (api) can transfer JS code to client to extend its functionality
6. Uniform Interface Constraint
   - API and API consumers share one single technical interface. Since typically working with HTTP protocol, interface should be seen as a combination of resource URIs, HTTP methods, and HTTP media types
   - Subconstraints:
     1. Identification of resources
        - Individual resources are identified in requests using URIs, and those resources are conceptually separate from their representation
        - Representation is the JSON or XML sent back in the response
     2. Manipulation of resources through representations
        - Representation plus metadata should be sufficient information to modify or delete the resource (permissions assumed)
     3. Self-descriptive messages
        - Each message must include enough information to describe how to process the message

REST often uses HTTP protocol, but doesn't have to.

**What is http protocol?**

1. A protocol that allows fetching of resources
   - A protocol is just a set of rules that define how data is exchanged within or between computers
2. Foundation of any data exchange on the web
3. Client-server protocol
   - Requests initiated by recipient
   - Clients and servers communicate by exchanging individual messages as opposed to a stream of data
   - Messages sent by client are called requests
   - Messages sent by server are called responses

**What are the http methods?**  
-> Different actions can use the same URI - it's up to the verb(method) to decide what happens.

1. GET
   - Used for reading resources
   - No request payload
   - Sample URI: api/employees, api/employees/{employeeId}
   - Response payloads: employees collection, single employee
2. POST
   - Used for creating a resource
   - Request payload: single employee
   - Sample URI: api/employees
   - Response payload: single employee
3. PUT
   - Used for updating resources (full updates)
   - Request payload: single employee
     - Should include all fields related to the employee. If a value is missing, default values should apply
   - Sample URI: api/employees/{employeeId}
   - Response payload: single updated employee or empty
4. PATCH
   - Used for partial updates
   - Request payload: JSON patch document on employee
   - Sample URI: /api/employees/{employeeId}
   - Response payload: single updated employee or empty
5. DELETE
   - Used to remove a resource
   - Request payload: none
   - Sample URI: /api/employees/{employeeId}
   - Response payload: none
6. HEAD
   - Identical to GET, but no response body should be returned
   - Used to obtain info on resource, like testing if a resource exists
7. OPTIONS
   - Represents a request for info about the communication options available on that URI
   - Will tell us whether or not we can GET/POST/DELETE the resource
   - Options are typically in response headers

**What are status codes?**  
-> Status codes tell the consumer of the API whether or not the request worked out as expected, and what is responsible for a failed req

1. Level 200: Success
   - 200: OK
   - 201: Created (resource created)
   - 204: Request OK, but no content returned
2. Level 400: Client mistakes
   - 400: Bad request, req sent to the server is wrong
   - 401: Unauthorized, no or invalid authentication details were provided
   - 403: Forbidden, authentication succeeded but user doesn't have access to resource
   - 404: Requested resource doesn't exist
   - 405: Method not allowed, like trying to send a req to a URI with POST method, but only GET is implemented
   - 406: Not acceptable, a consumer might request XML media type while API only supports JSON
   - 409: Conflict, like a conflict in request vs current state of resource (i.e. editing a version of a resource which has been renewed since editing started); also used when trying to create a resource that already exists.
   - 415: Unsupported media type, sort of opposite of 406
   - 422: Unprocessable entity, semantic mistakes, usually related to validation
3. Level 500: Server mistakes
   - 500: Internal server error, server made the mistake and client can't do anything about it

**Other things that are important:**  
-> Naming conventions

1. Use nouns, not actions
   - A RESTful URI should refer to a resource that is a thing, i.e. api/employees instead of api/getemployees
   - using nouns conveys meaning - it should describe resource
   - keep it consistent and predictable
2. Represent hierarchy when naming resources - i.e. api/departments/id/employees

-> Unchanging URIs

1. Resource URIs should remain same even if back end changes.
   - this is a good reason to not use real database IDs-what if someone has bookmarked api/departments/200?
   - can use GUIDs instead

**Richardson Maturity Model**  
-> Grades APIs by their RESTful maturity

1. Level 0: The swamp of plain old XML
   - HTTP protocol is used for remote interaction, but the rest of the protocol isn't used how it should be.
   - Examples are SOAP or other RPCs
   - Basically, HTTP is only used to transport
   - Example: Sending a POST req to /host/api to get info, and sending a POST req to same URI for a real POST. Info related to the resource you want is sent within XML body.
2. Level 1: Resources
   - Each resource is mapped to a URI, but still only one HTTP method used.
   - Example: /host/api/employees URI, and /host/api/employees/id URI, but API only uses a POST method
3. Level 2: Verbs
   - Correct HTTP verbs are used
   - Correct status codes are used
4. Level 3: Hypermedia
   - Level 3 is the bare minimum for an API to be considered truly RESTful
   - API supports HATEOAS
   - Example: a GET req to /api/employees would return a list of employees and code/links that drive application state (hypermedia)

**To be truly RESTful, must implement HATEOAS:**

1. Hypermedia as the engine of application state
   - Hypermedia is a generalization of hypertext (links)
   - Drives how to consume, use the API
   - Allows for a self-documenting API
   - Example: User asks API for list of departments, API sends back the list along with links for what the user can do next - there could be a link that says, 'Show all employees for this department' or 'Edit this department'
2. Without HATEOAS, intrinsic knowledge of the API contract is required, often obtained through documentation
   - Consumers have to know too much
   - A rule change or addition breaks consumers of the API
   - API cannot evolve separately
3. Links to what options are available would be put in the response body, using method/rel/href attributes
