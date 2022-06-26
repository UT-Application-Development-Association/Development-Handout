# Web Programming Overview

> Pre-requisite courses: CSC108/CSC148/CSC110/CSC111, CSC207
> Relevant courses: CSC309, CSC301

## Basic of the Basics

> Relevant courses: CSC301

OK, after you write your great machine learning models or programs (like what you made in CSC148/CSC111/CSC207), how can other people access your services?

***WEB/INTERNET!***

## Client

### Popular frameworks

- React
- Vue
- Angular

## Server

### Popular frameworks used in UTADA

- Python: Flask
- NodeJS: Express.js
- Java: Spring/SpringBoot

### Database

> Relevant courses: CSC343

How do we keep the data then, so that we can have them forever? To ensure the ***persistency*** of data, we need to write data to files. For example, in daily life we will write notes on a paper, or write to Word documents, Google Docs, or Excels. But these formats are not efficient enough, it would take minutes to find the information you want. Luckily, there are ***databases***, which are organized collection of data. They are optimized for **storing, querying, securing, and manipulating data with persistency**. We will use ***Database Management Systems*** (DBMS) to access databases and do the operations (we actually often refer to DBMS when we say database).

Databases can be hosted by database servers, which may run on your local machine or on cloud clusters.

During web-app development, we usually use a framework to do the jobs.

- Python: SQLAlchemy
- Java: JDBC, MyBatis
- Node.js: mongoose

#### Relational/SQL

Keywords: structured, predefined schema, vertically-scalable, table-based, ACID.

You will use *Structured Query Language* to manipulate data. For example:

```SQL
SELECT name, gpa FROM Students WHERE age=20;

INSERT INTO Students (name, age, gpa)
VALUES ('Tom', 20, 3.5);

UPDATE Students SET age=20, gpa=3.7 WHERE name='Jerry';

DELETE FROM Students WHERE name='Jerry' AND age=19;

SELECT * FROM Students JOIN Grades 
ON Students.name=Grades.student_name;
```

##### Typical SQL databases

MySQL, PostgreSQL, SQLite

#### Non-relational/NoSQL

Keywords: no strict structures, dynamic schemas, horizontally-scalable, document-based, BASE.

##### Typical NoSQL databases

MongoDB, DynamoDB

### More topics on backend

How can we make our server better?

- Caching: faster
  - Redis
- Security
  - Authentication
- Message Queue
- WebSockets: make a chatroom!

## What's more?

That's the basics! You are ready to make your first web-app, but this is far from the end (maybe not enough at all).

- DevOps
  - CI/CD
- Microservices
- Cloud Services
  - AWS
- Proxy and Reverse Proxy
  - Load Balancing
    - Nginx
- Team Collaboration
  - Agile

## Protocols

> Relevant courses: CSC309, CSC207, CSC369

These are not urgent for you to make your first web-app, but they are essential to your career development.

We need protocols to ensure data can successfully flow between the two agents and both agents can read the data.

### IP (Network layer)

How you find a server, and how the server finds you. This is also relevant to your [domain name](https://en.wikipedia.org/wiki/Domain_Name_System).

### TCP (Transport layer)

Say "Hi" and "Bye".

### TLS/SSL (Transport layer)

Keep your connection safe and private. This is how `https://your-domain.com` gets its `s`, in comparison to `http`

### HTTP/HTTPS (Application layer)

What data to send.

## Useful links

- Roadmaps
  - [Frontend](https://roadmap.sh/frontend)
  - [Backend](https://roadmap.sh/backend)

## References
