# Chama The App - Assignment from Felipe Zini

Ok, let's start to try to explain my code (or architecture).  
First of all, I want to say that I'm not good in English, for writing and listening it's ok, but for talk I don't have practice.  

## Production Code
I let the code published on my personal Azure account, this mean that some keys maybe are in the config files in the source, please do not use them, I'm not a rich man.  
The Swagger URL: https://chamatheappservice.azurewebsites.net/swagger

I implemented two version of the API, you can switch like image below:  
- v1: Enrollment is synchronous
  - StatusCode: 201 Created
- v2: Enrollment is asynchronous
  - StatusCode: 202 Accepted

On Azure I use the following services:
- App Service for WebApi hosting
- BlobStorage for Queue feature (Can be replaced for other services, like Azure ServiceBus)
- Cosmos DB (mongoDb) to save report data not in relational form
- SQL Azure to relational software data
- Azure Functions for asynchronous processing
  - Enrollment process
  - Reports create course

I work with Azure in 2012~2014 a lot of service were included since I stop use, so maybe the evolution of the code is start work with others cloud services.

## Wrapping up

### Architecture
I chose work with .NET Core 2.1 and ASP Net WebApi package, for dependency injection, I didn't see the need to change the official Microsoft framework, to work with data I chose Entity Framework Core, but I like (and sometimes I prefer) to work with Micro ORM like Massive, Dapper or SimpleData, in the application I chose a simple Repository Pattern to manage and retrieve data.

I used a library called [MediaR](https://github.com/jbogard/MediatR) to work with Mediator pattern and send commands and events between controller and features.
For solution I use [Feature Folders](https://github.com/ardalis/OrganizingAspNetCore/tree/master/src/WithFeatureFolders) to organize my code, I like this approach to separate my classes more clearly.

To solve API I used part of REST technic working with properly StatusCode for each request, for scaling I put a message into a queue to be processed and for querying I implement a simple CQRS to aggregate data.

### Bonus
The biggest problem was find a Azure solution to plug into application, another one was a concurrency problem (that I believe it's not solved). To solve that I think that I need a central point to manage how many seats are avaliable in each courses.
