# Lab 13: Referer-Based Access Control

**Link:** https://portswigger.net/web-security/access-control/lab-referer-based-access-control

## Goal
Log in as `wiener` and promote yourself to administrator by exploiting a flaw in how access control relies on the `Referer` header.

## What's the problem
Instead of properly checking whether the logged-in user is actually an admin, the app decides access based on the `Referer` header — basically trusting that if a request "came from" the admin panel page, the user must be allowed to perform the action. But the `Referer` header is fully controlled by the client, so it can be faked.

## Step 1: Explore as admin
1. Log in using:
   - **Username:** administrator
   - **Password:** admin
2. Open **Burp Suite** and turn on request interception (Proxy tab).
3. Go to the admin panel and click "Upgrade" on a test user.
4. In Burp's history, find this upgrade request — check its headers. Notice it includes something like:
Referer: https://<lab-id>.web-security-academy.net/admin-roles?username=some-user
5. 5. Note the exact request format and the `Referer` value used.
6. Log out of the admin account.

## Step 2: Exploit as a normal user
7. Log in using:
   - **Username:** wiener
   - **Password:** peter
8. In Burp, capture a request made while logged in as wiener (to get your current session cookie).
9. Take the earlier admin "upgrade" request, send it to **Repeater**.
10. Replace the session cookie in that request with your `wiener` session cookie.
11. Change the `username` parameter to `wiener`.
12. Make sure the `Referer` header still points to the admin panel page (e.g. `Referer: https://<lab-id>.web-security-academy.net/admin-roles`) — fake it if it's missing.
13. Send the request — since the app only checks the `Referer` header (not your actual role), the upgrade goes through.
14. Refresh the site — you are now an administrator.
15. Done! Lab solved.

## What we learn
The `Referer` header is sent by the browser and can be freely modified or spoofed by the client — it should never be used as a security control to decide who is authorized to do something. Real access control must verify the user's actual identity and role on the server side (using an authenticated session), not trust where a request claims to have come from.
