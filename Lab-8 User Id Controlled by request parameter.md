# Lab 8: User ID Controlled by Request Parameter with Password Disclosure

**Link:** https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-password-disclosure

## Goal
Get the administrator's password, log in as admin, and delete the user `carlos`.

## What's the problem?
The account page shows the current user's password in a masked (dots/asterisks) input field — but the actual password value is still present in the page's HTML source. Combined with the fact that the `id` parameter can be changed to load another user's account page, this leaks other users' passwords too, including the administrator's.

## Login credentials
- **Username:** wiener
- **Password:** peter

## How to solve it (step by step)

1. Log in to the lab using `wiener` / `peter`.
2. Go to **My Account** — notice the password field is masked but prefilled.
3. Open **Burp Suite** and turn on request interception (Proxy tab), or simply use browser **Inspect (View Page Source)**.
4. Reload your account page and capture the request, e.g. `GET /my-account?id=wiener`.
5. Send this request to **Repeater**.
6. Change the parameter from `id=wiener` to `id=administrator`.
7. Send the request and look at the raw HTML response.
8. Find the password `<input>` field — even though it displays as masked dots on screen, the actual `value="..."` attribute in the HTML contains the real administrator password in plain text.
9. Copy that password.
10. Log out, then log in again using:
    - **Username:** administrator
    - **Password:** (the password you just copied)
11. Once logged in as admin, open the admin panel.
12. Find the user `carlos` and delete the account.
13. Done! Lab solved.

## What we learn
Masking a password visually in the browser (using dots) is not real protection — if the actual value is still sent in the page's HTML source, it can be read directly. Sensitive fields like passwords should never be pre-filled or included in a response at all. On top of that, this lab again shows how allowing users to freely change an `id` parameter exposes other users' private data (horizontal privilege escalation).
