Peacock Core Video SDK Coding Challenge

Write a simple IoC library in either Typescript or Javascript

Steps I take to make the library great to use:

Step 1 - Installing the OWJA! IoC library
npm install --save-dev @owja/ioc

Step 2 - Creating symbols for dependencies
Create the folder services and add the new file services/types.ts

Step 3- Example services
Create example services.

File services/my-service.ts
File services/my-other-service.ts

Step 4 - Creating a container
Create a container to bind our dependencies to. Let's create the file services/container.ts

Step 5 - Injecting dependencies
create a example.ts file
If we run this example we should see the content of our example services.

The dependencies (services) will injected on the first call. This means if you rebind the service after accessing the properties of the Example class, it will not resolve a new service. If you want a new service each time you call example.myService you have to add the NOCACHE tag.

Step 6 - Unit testing using IoC
Create mocks in test/my-other-service-mock.ts
Within the tests we can snapshot and restore a container. We are able to make multiple snapshots in a row too.

File example.test.ts

Step 7 - code for further collaborative development
A suggestion for further collaborative development could be how to support type-safe injection. The proposed changes are backward compatible.

We can create a token that is bound to a type like this:

const serviceToken = token<SomeService>("SomeService");
Now we can register a service using this token similar to how we could register using a symbol:

container.bind(serviceToken).toFactory(() => new SomeService());
(Note that the type parameter of bind was inferred from token)

However attempting to return value of a type that is not assignable to SomeService is a type error now:

container.bind(serviceToken).toFactory(() => new SomeOtherService()); // Fails at compile time
When retrieving also we benefit from type inference (and type safety):

const service = container.get(serviceToken); // service inferred as SomeService
The above applies to all modes of bind (value, factory, class etc.) and inject (@inject, wire, resolve etc.)
