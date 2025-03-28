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
The vulnerability in `insecure.ts` is the lack of authentication and logging, which allows any user to send or retrieve messages without verification. This makes the system susceptible to **repudiation attacks**, where a user can perform actions and later deny having done so, since there is no way to trace or prove who initiated the request.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.
The vulnerability is addressed in `secure.ts` by adding request logging and simulated authentication. All actions, such as sending or retrieving messages, are logged with timestamps and IP addresses, creating a clear audit trail. This ensures that user actions can be traced, making it harder for anyone to deny their activity and effectively preventing repudiation.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
The secure version uses the **Interceptor design pattern** through middleware to address the vulnerability. This pattern allows incoming requests to be intercepted and processed before reaching the main application logic. In `secure.ts`, a logging middleware captures details like the request method, URL, timestamp, and IP address, and writes them to a log file. This ensures that every user action is recorded, enabling traceability and preventing users from denying their actions, which effectively mitigates the risk of repudiation attacks.