# REST
The RESTful design is one of the most popular ways of the backend design for a web app.  This page serves as a short summary for the introduction to the REST API.

List of contents in this post:
- [Resource](#Resource)
- [URI Design](#URI-Design)
- [Resource Format](#Resource-Format)
- [Resource Action](#Resource-Action(Resource-Request))
- [Path Parameter And Query Parameter](#Path-Parameter-And-Query-Parameter)
- [Status Code](#Status-Code)

## Resource
Informally, the REST API concentrates on the **web resource** which is the **providing service** of the web app.  Other users can gain the service by using the REST API provided by a web app.  The REST API (or the web service) is presented to the users by the **URI (the Unique Resource Identifier)**. This is similar to the URL with slight differences which will not be discussed in this Wiki.

## URI Design
There are several conventions for designing a web URI.

A well-written reference can be found [here](https://blog.restcase.com/7-rules-for-rest-api-uri-design/)

## Resource Format
Resource format defines how does service presents the resource.  For instance, JSON, XML, YAML, etc.

## Resource Action(Resource Request)
Resource actions define the available actions for the web resource, those actions are borrowed from HTTP methods.

Here is a list of commonly-used actions in the REST design.
- GET: Obtain the resource

- POST: Create a resource

- PUT:  Create or Update a resource

- PATCH: Update a resource

- Delete: Delete a resource

Some keynotes here:
1. GET, HEAD, PUT, and DELETE are known as **idempotent** methods meaning that apply them multiple times to a resource results in the same state change of the resources as applying them once(i.e. if I accidentally call PUT 10 times to create the same resource only one resource will be created instead of 10).  All safe methods are idempotent.

2. PUT and POST both can be used to create the resource with different circumstances:
    - POST is used to create the subordinate(child) resource of an existing resource(parent)

    - PUT can be used to create both

   An example borrowed from AWS design.  In S3, there are two types of resources: Bucket, and Object.  Every Object is a subordinate resource of the Bucket.  To create a bucket I have to use PUT `PUT /BucketName`.

   I have two ways to create an object.
    1. `PUT /BucketName/ObjectName`
    2. `POST /BucketName`

   The major distinction here is that PUT requires a **specific URI** just the same as the GET.  Nevertheless, the POST only knows the **parent resource**.

3. PUT and Patch both can be used for updating:
    - Use PATCH when you need to change **part of the resource**

    - Always provide the **complete resource** using the PUT

## Path Parameter And Query Parameter
There are two types of parameters in the URI.  Path parameter and query parameter.  `/home/{department}/courses/?offering={season}`.  
The **department** along the URI is the path parameter whereas the **offering** is the query parameter.
- Path parameter: To **locate or identify** the resource

- Query parameter: To **filter or sort** the resource

## Status Code
After the action is done, we need a way to tell the user whether the service is performed or any potential errors.  This is achieved through the status code.

There are three categories of status code
- 200-level: The 200 stands for the success

- 400-level: 400 stands for the client error
    - 400 Bad Request: This happens when the request missing mandatory fields or having syntax errors in the request which the server cannot perform the service

    - 401 Unauthorized: This happens when the user requests the service without authorization.

    - 403 Forbidden: The request is denied by the server potentially due to missing SSL, sending from an IP on the blacklist, the certificate attached is invalid, etc

    - 404 Resource Not Found: The client is trying to access a resource that is not provided by the server

    - 405 Method Not Allowed: The client is sending a request which is not provided for the resource

- 500-level: 500 stands for the server error which is a serious problem for the developers need to fix.  Things like server run time error or any error that happened during server process the request belong to 500.

The rule of thumb is telling the customer the error status as precise as possible, thus, they can now how to fix the problem.  Be thoughtful when you process the request so 500 can be avoided.  Let your customer pay the cost of their miss, not the developers. 



