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
The spoofing vulnerability in `insecure.ts` arises from the lack of authentication and trust placed on user input to set session values. Specifically, the `/register` endpoint allows users to set their session's `user` field simply by submitting a name through a form, without any verification. This means an attacker can submit the name "Admin" and gain elevated privileges, as the `/sensitive` endpoint checks for `req.session.user === 'Admin'` to authorize access to sensitive operations. Since there's no actual login or authentication mechanism in place, this makes it easy for anyone to impersonate an admin. Additionally, the session cookie is configured with `httpOnly: false`, making it accessible via client-side JavaScript and increasing the risk of session hijacking through XSS attacks.

2. Briefly explain different ways in which vulnerability can be exploited.
- Session spoofing: Attacker submits "Admin" in the form to gain unauthorized access to sensitive routes.
- Session fixation: Attacker sets up a session with "Admin" and tricks a user into using it.
- Cookie theft: With httpOnly set to false, attackers can steal session cookies via JavaScript.
- Role guessing: No validation allows attackers to try different role names to gain access.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
The `secure.ts` version mitigates the spoofing vulnerability present in `insecure.ts` by using safer session cookie settings and better configuration practices. Specifically, it sets the `httpOnly` flag to `true`, which prevents JavaScript from accessing session cookies and reduces the risk of cookie theft via XSS. It also enables `sameSite`, which helps protect against CSRF attacks. Unlike `insecure.ts`, the session secret is passed securely via environment arguments instead of being hardcoded. While both versions still lack proper authentication, `secure.ts` makes session hijacking and manipulation significantly harder due to its stricter session configuration.