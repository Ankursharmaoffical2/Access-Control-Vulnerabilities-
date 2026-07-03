# Lab 5: User ID Controlled by Request Parameter

**Link:** https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter

## Goal
Get the API key belonging to the user `carlos` and submit it as the solution.

## What's the problem?
This is a **horizontal privilege escalation** issue. The account page loads a specific user's data based on a `id` parameter in the URL/request. The server doesn't check whether the logged-in user actually owns that ID — so anyone can change the ID to view another user's data.

## Login credentials
- **Username:** wiener
- **Password:** peter

## How to solve it (step by step)

1. Log in to the lab using `wiener` / `peter`.
2. Go to **My Account** — you'll see your own API key here, and the URL/request will contain something like `id=wiener`.
3. Open **Burp Suite** and turn on request interception (Proxy tab).
4. Reload the account page so Burp captures the request (e.g. `GET /my-account?id=wiener`).
5. Send this request to **Repeater**.
6. In Repeater, change the parameter value from `id=wiener` to `id=carlos`.
7. Send the request — the response now shows carlos's account page, including his API key.
8. Copy carlos's API key.
9. Submit that API key as the solution to solve the lab.

## What we learn
An application must always verify that the logged-in user is actually authorized to see the data they're requesting — not just trust an ID sent in the request. Relying on a parameter like `id=username` without an ownership check lets any user view or access another user's private data (horizontal privilege escalation).
