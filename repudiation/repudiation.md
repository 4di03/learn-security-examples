# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.

In **insecure.ts** the user is allowed to send messages with no logging of when they were sent. If a certain message causes an issue, it is hard to trace exactly which one did it as we could never find the message that was appended right before the issue.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.

**secure.ts** incorporates logging of the time stamps of each action on the server, allowing us to trace the cause of certain issues by looking at the events that happened right before them.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?

The pubsub pattern as the logs are writtne to a file stream. The file stream contains subscribers which react to write events by writing to to the file, and the handlers publish write events (which are log messages). 