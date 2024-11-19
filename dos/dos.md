# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.

One key vulnerability is that there is no sanitization of the id parameter passed in the request, so if there is an invalid id string passed, it will throw a query error which will crash the server. Another issue is that the server can get overloaded with requests and run out of memory or threads if an attacker decides to send too many.

2. Briefly explain how a malicious attacker can exploit them.


An attacker can make a GET request with an empty string as an id parameter and crash the server with a query error. Also, an attacker could send thousands of concurrent requests to the server, overloading its memory and threadpool and causing it to crash.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?

**secure.ts** implements error handling in the handler for the GET /userinfo route to just return an error message with a 500 response if there is some error in executing the query. This means that even if the user sends an invalid id like an empty string, the server wont go down as the error will be caught. Alsom there is a rate limiter which limits the number of incoming requests to one every 5 seconds, which prevents the server form being overloaded by requests.