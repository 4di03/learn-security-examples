# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

The key issue here is that code injection is allowed in this input. The user can put HTML code in the input and it is just placed without validation on the webpage. If this contrains
javascript code, it will execute that code at runtime.

2. Briefly explain how a malicious attacker can exploit them.

The text input is not being validated and it allows the user to put in HTML code which can display malicious links. This allows the attacker to plant malicious links in the website which can open up further attacks on users, or they could execute some code to potentially destroy or expose some sensitive data on the server.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?

In **secure.ts** , the data entered in the form which is processed by the /register endpoint is santized using the escapeHtml function. This will replace html tags and other special characters with other symbols to prevent HTML or javascript injection.

