# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.

The session token is set in cookies, which can be programmatically read by a spoofing attack such as the malicious webpage link. The attackers can then append this token to the REST url when making requests to the server to gain access to the user's session, potentially exposing sensitive data to the attacker.


2. Briefly explain different ways in which vulnerability can be exploited.

One way is described as above, where the attacker is able to swipe session token data from the cookies programmatically and use that to gain access to the server's resources pretending to be the user.

Another method is via a CSRF attack. Once a cooke is set in HTTP, it is sent with all subsequent requests, so subsequent malicious requests (even from different domains entirely ) would be able to take advantage of this cookie to get access to privileged part of the server requiring the token. Therefore teh attacker could take advantage of the set cookie to forge the request from a different domain and gain access to sensitive data.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

**secure.ts** initalizes sessions with httpOnly and sameSite as true. httpOnly prevents cookies form being read or written programmatically. sameSite only allows cookies from the same domain to be sent to the server, so if the cookie is somehow stolen or forged and sent form another domain, it will not be accepted.