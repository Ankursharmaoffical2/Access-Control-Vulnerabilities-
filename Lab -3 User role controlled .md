# Lab 3: User Role Controlled by Request Parameter

**Link:** https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter

## Goal
Delete the user `carlos` by accessing the admin panel at `/admin`.

## What's the problem?
The admin panel checks if a user is an admin using a cookie — but this cookie can be edited by the user, since it's not properly protected on the server side. If you just change the cookie value, the server wrongly believes you're an admin.

## Login credentials
Use the given account to log in first:
- **Username:** wiener
- **Password:** peter

## How to solve it (step by step)

1. Log in to the lab using `wiener` / `peter`.
2. Open **Burp Suite** and turn on request interception (Proxy tab).
3. Browse the site normally so Burp captures your traffic, especially the request to `/my-account?id=wiener`.
4. In this captured request, find a cookie like `Admin=false`.
5. Send this request to **Repeater**.
6. In Repeater, change the cookie value from `Admin=false` to `Admin=true`.
7. Send the request — the response should now show the admin panel content.
8. To make this change take effect in your browser too, open **Developer Tools → Application/Storage → Cookies** (or use a Cookie Editor extension).
9. Find the `Admin` cookie and change its value from `false` to `true`.
10. Refresh the page and go to `/admin` — the admin panel now opens.
11. Find the user `carlos` and delete the account.
12. Done! Lab solved.

## What we learn
Never trust data that comes from the client side — like cookies — to decide who is an admin. Cookies can be freely read and edited by users. Role and permission checks (like "is this user an admin?") must always be verified on the server, using a trusted session, not a value the browser sends.
