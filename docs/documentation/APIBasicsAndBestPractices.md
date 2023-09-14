# Overview

This guide provides basic information and best practices to help you successfully integrate your application with our APIs.

The CardPointe platform provides a library of application programming interfaces (APIs) that allow you to integrate various services within your application. Each API uses a standard set of protocols to allow your application to interact with these services. Before you begin to integrate your application with our APIs, you should understand the basic concepts and the best practices for implementing them.

## API Development Basics

Before you get started, you should have a functional understanding of the following concepts:

- [HTTP RESTful web methods](#understanding-http-restful-web-methods)
- [JavaScript object notation (JSON)](#understanding-json)

### Understanding HTTP RESTful Web Methods

RESTful APIs provide HTTP methods to allow a client application to perform create, read, update, and delete (CRUD) functions for shared resources across the web.

> If you're new to working with RESTful APIs or HTTP methods, you can find detailed technical information and additional resources at https://restfulapi.net/http-methods/.

In general, the following HTTP request methods are used to perform these CRUD functions:

| Request Method | Function |
| -------------- | -------- |
| POST | Create a resource |
| PUT | Update a resource |
| GET | Retrieve a resource |
| DELETE | Delete a resource |

Each API supports standard HTTP request types; see the API documentation for details on the specific methods supported for each API. 

### Understanding JSON

JavaScript Object Notation (JSON), is a simple, lightweight protocol for sending and receiving data.

> If you're new to working with JSON, you can find detailed technical information and additional resources at https://www.json.org.

 All of our APIs use JSON objects, arrays, and key/value pairs to transmit data to and from your application.

<!-- theme: danger -->
>All updates to the APIs are backwards compatible, and we reserve the right to add new properties (key/value pairs) within the JSON response. 
>
> Because the exact response content is subject to change, it is a best practice to develop client applications that dynamically accept and parse all JSON response properties, rather than coding for the static position of specific properties within the response data. If your application expects a specific value to be returned in an exact position within the response object, you might encounter errors and downtime in the event that we introduce a new values to the response object.
>
> See [Ensuring backwards compatibility](#ensuring-backwards-compatibility), below, for more information.

<!-- theme: warning -->
> When writing your application, you can use the JSON Valdiator at https://jsonlint.com to validate the syntax of your JSON requests. 

## Best Practices and Recommendations

In addition to understanding the basics, we strongly recommend that you adhere to the following best practices to ensure a successful integration:

- [Ensuring backwards compatibility](#ensuring-backwards-compatibility)
- [Acceptance and regression testing](#accepting-and-regression-testing)

### Ensuring Backwards Compatibility

In order to develop new features and enhancements, and to remain compliant with industry standards, we reserve the right to make the following changes to our APIs:

- Add new API interfaces to API services.
- Add new methods to an API interface.
- Add new HTTP bindings to existing methods.
- Add new parameters to existing request messages.
- Add new fields to existing response messages.
- Add new values to existing enumerations.
- Add new output-only resource fields.

These changes are considered backwards compatible; If your application is designed to dynamically parse and handle JSON response objects (for example, to ignore unknown values), these API changes should not result in any errors or downtime for your application. However, if your application is designed to retrieve specific name/value pairs from exact locations in a response object, your application will encounter errors when new fields or values are added to the response.

<!-- theme: danger -->
> The JSON standard does not assign any significance to the ordering of key/value pairs, and we do not guarantee the order of key/value pairs in response messages for any of our APIs.

### Accepting and Regression Testing

Whether you are developing a new application or supporting an existing integration, testing should be considered a critical task in your workflow. 

We provide access to a user acceptance testing (UAT) environment, which allows you to test your application using sample data and emulated responses. Before your application can be considered production-ready, you must complete your acceptance testing in this environment to ensure that the application is functioning as intended.

See [Testing your Integration](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md#testing-your-integration) for more information on testing in the UAT environment.

Additionally, it is a common best practice to schedule regular regression testing in your development and support lifecycle. We periodically introduces new features and [backwards compatible](#ensuring-backwards-compatibility) API changes, which are described in the What's New? topic in the API documentation. 

<!-- theme: warning -->
> Additionally, you can subscribe to our [Statuspage](https://status.cardconnect.com/) to receive alerts for new releases and service incidents. 

By scheduling regular regression testing, you can help to ensure that your application is correctly integrating and making the most of new features and updates to our services.
