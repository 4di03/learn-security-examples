# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

**insecure.ts** allows any user that knows the admin user id to update the role of that admin. If this id was leaked somehow, than a malicious attacker could easily change the role of the admin.

2. Briefly explain how a malicious attacker can exploit them.

The malicious attacker would simply input an id of 1 and change the role to whatever they please.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?

**secure.ts** verifies the identitiy of the request maker by keeping track of the userId in the user's session and using that to determine privileges. This way, only the user who's session has a userId of 1 can actually change roles. Since new requests will have new session ids, only the admin with the first userId will ever be able to change the roles.