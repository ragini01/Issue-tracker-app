### - Realtime Client/Server issue tracking statistics
The objective of this project is to design and implement a Client/Server application that simulates the creation and completion of user-stories and tasks, showing real-time statistics.

#### Tech Stack Used:
MEAN stack
     MongoDB—MongoDB used as the database to store the data for the stories and tasks. The data model include collections for stories and tasks, with fields for the story/task name, status, estimate, actual time spent, and other relevant information.
     Express.js—Express.js used to create the server-side API to handle incoming requests from the client, process the data, and send real-time updates back to the client. The API include endpoints to create a new story, get a list of open stories, get real-time updates on story/task processing status, and other relevant functionalities.
     Angular—Angular used to create the client-side interface to display the data and receive real-time updates. The interface includes a dashboard to display the real-time statistics, a form to create new stories, and other relevant functionalities.
     Node.js—Node.js used to run the server-side code and handle incoming requests from the client.


#### Architecture

This describes the main components of the system, their interactions, and the flow of data between them. Here's a brief overview of each component:

1. Client: The user interface that sends requests to the server to create stories and tasks and receives real-time updates about the processing statistics.

2. Server: The central component that receives requests from the client, processes them, and sends real-time updates to the client.

3. Database: The persistent storage where the server stores the data about stories and tasks.

4. Workers: The threads or processes that the server uses to process the tasks.

5. Cache: A cache layer is used to improve performance by storing frequently accessed data in memory. This can reduce the number of requests to the database and improve response times.

6. WebSocket: The protocol that the server uses to send real-time updates to the client.

7. Web Worker: A separate thread that the client uses to offload the processing of incoming real-time updates, to make the user interface more responsive.

#### UI Technology

    For the user interface, we will be using Angular, which is a popular JavaScript framework for building web applications. Angular has a built-in template engine, and it allows us to easily create reusable components and services. We will also be using Angular Material, which is a UI component library that provides pre-built components such as buttons, forms, and tables.
    
    1. UI will display the following information
        a. Stories produced per sec
        b. Stories completed per sec
        c. Number of open stories
        d. Number of completed stories

    2. It also allows users to add new stories or delete stories

#### API and Data Model Design

    For the API, we will be using RESTful endpoints to handle CRUD operations for stories and tasks. Here are the API endpoints we will be using:

    1. POST /api/stories - Create a new story
    2. GET /api/stories - Retrieve all stories
    3. GET /api/tasks - Retrieve all tasks
    
    For the data model, we will have two collections in our MongoDB database: stories and tasks. Each story will have an array of tasks, and each task will belong to a single story. Here are the fields we will include in each collection:

    Story Collection:

    id: string (auto-generated)
    title: string
    description: string
    status: string (Open or Done or In Progress), default: Open
    tasks: array of task IDs

    Task Collection:

    id: string (auto-generated)
    title: string
    description: string
    status: string (Open or Done or In Progress), default: Open
    estimate: number (in seconds)
    storyId: string (ID of the story the task belongs to)
    timeSpent: number (in seconds)

#### Real-time Aspect of Informing the Client About Changes

    To keep the client informed about changes in real-time, we will be using web sockets. We will set up a separate web socket connection between the client and server, and the server will send real-time updates to the client as stories and tasks are created, updated, or completed. When a story is created or completed, the server will emit an event over the web socket, and the client will receive the event and update the UI accordingly.

#### Data Persistence

    To ensure that data is persisted even if the server is restarted, we will be using MongoDB as our database. MongoDB is a NoSQL database that can handle JSON-like documents, making it a good choice for our data model. We will set up our MongoDB database on a separate server, and the server application will connect to the database using Mongoose, which is a popular library for working with MongoDB in Node.js.

#### Rapid Prototype Development Stages

STAGE 1: BUILD STATIC SITE

The aims of this stage are
 To quickly figure out the layout
 To ensure that the user flow makes sense
At this point, you’re not concerned with a database or flashy effects on the user interface; all you want to do is create a working mockup of the main screens and journeys that a user will take through the application.

STAGE 2: DESIGN THE DATA MODEL AND CREATE THE DATABASE
The aims of this stage are
     To define a data model that reflects the requirements of the application
     To create a database to work with the model
The first part is defining the data model. Stepping back to a bird’s-eye view, what are the objects you need data about, how are the objects connected, and what data is held in them?
When you try to do this stage before building the static prototype, you’re dealing with abstract concepts and ideas. When you have a prototype, you can see what’s happening on different pages and what data is needed where. Suddenly, this stage becomes much easier. Almost unknown to you, you’ve done the hard thinking while building the static prototype.

STAGE 3: BUILD YOUR DATA API
After stages 1 and 2, you have a static site on one hand and a database on the other.
This stage and the next take the natural steps of linking them. The aim of stage 3 is
     To create a RESTful API that allows your application to interact with the database

STAGE 4: HOOK THE DATABASE INTO THE APPLICATION
When you get to this stage, you have a static application and an API exposing an interface to your database. The aim of this stage is
     To get your application to talk to your API
When this stage is complete, the application will look pretty much the same as it did before, but the data will be coming from the database. When it’s done, you’ll have a data-driven application!

STAGE 5: AUGMENT THE APPLICATION
This stage is all about embellishing the application with additional functionality. You might add authentication systems, data validation, or methods for displaying error messages to users. This stage could include adding more interactivity to the front end or tightening the business logic in the application.
The aims of this stage are
     To add finishing touches to your application
     To get the application ready for people to use


#### Hardware architecture

    1. Development hardware
    2. Production hardware: A common hardware architecture approach: one server to run the application code and API and a second, separate database server (A decoupled architecture using three servers: one for the database, one for the API, and one for the application code), (You can scale MEAN applications by having clusters of servers for each part of your entire application.)

#### Improvment or Alternate interesting implementation choices 

    1. One interesting implementation choice we can make is to use a queue system such as RabbitMQ or Apache Kafka for processing tasks and to ensure optimal performance. Instead of spawning a new worker for each incoming task, we can add the tasks to a queue, and then have a fixed number of workers that pull tasks from the queue and process them asynchronously using a pool of workers. This can help prevent the server from becoming overloaded with too many workers, and it can also ensure that tasks are processed in the order they were received. 
    2. Another interesting implementation choice we can make is to use a reactive programming approach for handling real-time updates. Reactive programming allows us to handle asynchronous events in a more intuitive way, and it can make it easier to manage complex data flows. We can use a library like RxJS to handle real-time updates from the server, and then use Angular's built-in Change Detection mechanism to update the UI accordingly.
    3. To handle a large number of incoming requests, we can use a load balancer like NGINX to distribute requests across multiple instances of the server.
    4. To ensure data consistency, we can use a distributed lock mechanism such as ZooKeeper or Redis lock to prevent multiple workers from processing the same story or task simultaneously.
    5. We can use Docker and Docker Compose to package and deploy this client/server issue tracking application in a containerized environment. Here's an example of how I might structure our Docker setup:
        a. Create a Dockerfile for server-side/backend application. This file should specify the base image, any necessary dependencies, and the commands required to start the server.
        b. Create another Dockerfile for client-side/frontend application. This file should specify the base image, any necessary dependencies, and the commands required to start the client.
        c. Create a Docker Compose file that defines the services for the application. This file should include the server, client, database, cache, workers, and any other necessary services. In the Docker Compose file, specify the environment variables required by application, such as the database connection string and the port numbers for the server and client.
        d. Use the Docker Compose command to build and run the services defined in the Compose file. This will create a containerized environment that includes all the necessary components of this application.
        e. Test application in the Docker environment to ensure that it works as expected. We may need to make adjustments to our configuration or code to ensure that it runs correctly in the containerized environment.

    6. More advanced features/functionalities can be added such as user authentication, notifications to alert users when there are updates to issues they are following or assigned to, search and filter functionality, integration with other tools like version control systems or CI/CD pipelines or other project managment tools, reporting and analytics will help users better understand trends and patterns in issue data.

