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
The main tampering vulnerability in `insecure.ts` is the ability for users to modify their session data without authentication. Since the `/register` endpoint sets `req.session.user` based purely on user input, an attacker can tamper with the session by submitting a privileged value like "Admin". This lets them access sensitive routes without any real verification. Although cookie settings are stricter (`httpOnly` and `sameSite: 'strict'`), the lack of input validation or role enforcement allows users to manipulate session state and escalate privileges, making the app vulnerable to logic tampering.

2. Briefly explain how a malicious attacker can exploit them.
A malicious attacker can exploit the tampering vulnerability by submitting `"Admin"` in the registration form, which sets their session's `user` value to "Admin". Since the app checks only this session value to authorize access to the `/sensitive` route, the attacker can gain unauthorized access and perform privileged actions without real admin credentials or authentication. This lets them bypass access controls using simple input manipulation.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
`secure.ts` avoids the vulnerabilities seen in `insecure.ts` by implementing key security measures. First, it sanitizes user input using `escapeHTML`, which prevents script injection and protects against XSS attacks. Second, the session cookie is set with `httpOnly: true` and `sameSite: 'strict'`, reducing the risk of session theft and CSRF attacks. While it still sets the `user` value from user input, the input is now cleaned, and the overall session configuration is more secure—making it harder for an attacker to tamper with session data or exploit trust in raw input.