# Lab 10: URL-Based Access Control Can Be Circumvented

**Link:** https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented

## Goal
Access the admin panel and delete the user `carlos`.

## What's the problem?
A front-end system (like a reverse proxy or load balancer) blocks direct external access to `/admin`. But the back-end framework trusts a special header called `X-Original-URL` to decide the actual path to process. Since the front-end only checks the real request path (not this header), an attacker can bypass the block by hiding the real destination inside the header instead.

## How to solve it (step by step)

1. Open the lab website.
2. Try visiting `/admin` directly — the front-end blocks it (you'll get a "Not Found" or "Forbidden" response).
3. Open **Burp Suite** and turn on request interception (Proxy tab).
4. Capture a normal request to the home page, e.g. `GET /`.
5. Send this request to **Repeater**.
6. Add a new header to the request: `X-Original-URL: /invalid`.
7. Send the request — you should get a **404 Not Found**, confirming the back-end is actually reading and using this header.
8. Now change the header to: `X-Original-URL: /admin`.
9. Send the request — this time you get access to the admin panel content, proving the bypass works.
10. To delete carlos directly, change the header to:
    `X-Original-URL: /admin/delete?username=carlos`
    (keep the actual request path as `GET /` while the header carries the real destination).
11. Send the request — the server processes it as if you accessed `/admin/delete?username=carlos`, and carlos gets deleted.
12. Done! Lab solved.

## What we learn
Blocking access based only on the front-facing URL path isn't enough if the back-end system trusts other means (like custom headers) to determine the real path. Front-end and back-end systems must be consistent — access control decisions should be enforced at the layer that actually processes the request, not assumed to be handled elsewhere.
