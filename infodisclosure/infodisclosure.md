# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

**insecure.ts** exposes a GET endpoint to fetch user data; however, the username paremeter accepts mongodb query parameters , which may allow attackers to execute mongodb queries to expose user data without actually knowing the usernames. This is an information disclosure vulernabilty as it allows attackers to expose private information without having proper credentials

2. Briefly explain how a malicious attacker can exploit them.

An attacker might input a mongo query such as {$ne : ''} in to the query parameter as done in the above request in order to find any user with a non-empty username, allowing them to see private account information of an account which they have no ownership of.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?

**secure.ts** prevents the attacker from injecting mongodb queries into the username parameter by first checking that the input has type string and then it replaces non alphanumeric characters with empty strings. This prevents attackers from injecting queries that relay on brackets, colons, or other special characters into the query, only allowing them to view a users information if they know the username.
