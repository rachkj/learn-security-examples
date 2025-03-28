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
The `insecure.ts` server is vulnerable to a Denial-of-Service (DoS) attack due to improper handling of user input in the `/userinfo` route. It directly uses the `id` query parameter in a MongoDB query without validating its type or format. If an attacker sends a malformed or unexpected input like `id[$ne]=`, Mongoose attempts to cast it to an ObjectId, which throws a CastError. Since this error is not caught using a try-catch block, it crashes the server, allowing attackers to repeatedly send such requests to make the application unavailable.

2. Briefly explain how a malicious attacker can exploit them.
A malicious attacker can exploit the vulnerability by sending a crafted request like `http://localhost:3000/userinfo?id[$ne]=`, which passes an invalid `_id` format to the database query. This triggers a CastError in Mongoose, and since the server lacks error handling, it crashes. By repeatedly sending such requests, the attacker can cause the server to crash continuously, resulting in a Denial-of-Service (DoS).

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?
The `secure.ts` file uses two key defensive techniques to prevent the DoS vulnerability. First, it wraps the database query in a try-catch block, so if an invalid or malicious `id` causes a Mongoose CastError, the server handles it gracefully without crashing. Second, it implements rate limiting using `express-rate-limit`, which restricts each IP to only one request every 5 seconds, reducing the impact of repeated malicious requests. Together, these measures protect the server from being overwhelmed or brought down by crafted inputs or high-frequency attacks.