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
The potential vulnerability in insecure.ts is a privilege escalation flaw where any user can submit a request with an admin userId and a new role to update another user's privileges. Since there's no real authentication or proper authorization, the server trusts user-submitted data to identify and authorize the action.

2. Briefly explain how a malicious attacker can exploit them.
A malicious attacker can exploit this by sending a POST request with userId set to an admin's ID and a newRole field, thereby tricking the server into thinking they are an admin and updating roles for themselves or others without having actual admin rights.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
In secure.ts, the vulnerability is prevented using session-based authentication. The server checks if the requester is logged in by verifying req.session.userId and ensures the logged-in user has an admin role before allowing any role update. This prevents unauthorized users from performing privileged actions, as they cannot spoof session data or elevate their privileges without proper login and role verification.