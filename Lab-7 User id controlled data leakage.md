# Lab 7: User ID Controlled by Request Parameter with Data Leakage in Redirect

**Link:** https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-data-leakage-in-redirect

## Goal
Get the API key belonging to the user `carlos` and submit it as the solution.

## What's the problem?
This lab looks like it fixed the access control issue on the surface — when you try to view another user's account, the app **redirects** you away (like it's blocking you). But the redirect response itself still contains the sensitive data in its body, before the redirect actually happens. So the leaked data can be seen even though you never "reach" the final page.

## Login credentials
- **Username:** wiener
- **Password:** peter

## How to solve it (step by step)

1. Log in to the lab using `wiener` / `peter`.
2. Go to **My Account** — note the URL/request, e.g. `GET /my-account?id=wiener`, and see your own API key on the page.
3. Open **Burp Suite** and turn on request interception (Proxy tab).
4. Reload your account page so Burp captures the request.
5. Send this request to **Repeater**.
6. Change the parameter from `id=wiener` to `id=carlos`.
7. Send the request — the browser would normally redirect you away, but in Repeater you can see the **full raw response**.
8. Look closely at the response body — even though it's a redirect, carlos's account details (including his API key) are still present in the body text.
9. Copy carlos's API key from that response

## What we learn
Redirecting a user away from unauthorized content is not enough — if sensitive data is already written into the response body before the redirect, it's still leaked. Proper access control must stop sensitive data from ever being included in the response, not just try to redirect the user afterward.
