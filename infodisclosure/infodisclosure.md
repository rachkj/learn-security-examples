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
Information Disclosure: The app sends the full user object (res.send(\User: ${user}`)`), which may include sensitive information like hashed passwords. This can leak internal schema details or confidential data.


2. Briefly explain how a malicious attacker can exploit them.
A malicious attacker can exploit these vulnerabilities by crafting a query string like `/userinfo?username[$ne]=null`, which uses a NoSQL injection to bypass the username check and retrieve any user. Since the response sends the entire user object, the attacker may gain access to sensitive data like usernames or hashed passwords. Additionally, lack of input validation allows attackers to send unexpected types or structures, and the server may log these payloads, potentially leaking harmful data or enabling further attacks.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
In `secure.ts`, several defensive techniques are used to prevent information disclosure:

1. **Input validation**: It checks if the `username` is a string, rejecting unexpected types early.
2. **Input sanitization**: Special characters are removed from the `username` to prevent NoSQL injection.
3. **Restricted response**: Unlike the insecure version, it avoids exposing sensitive internal details like the full user object or password in the response.
4. **Error handling**: It catches and logs database errors without leaking stack traces or internal details to the user.