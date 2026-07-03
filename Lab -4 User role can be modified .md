# Lab 4: User Role Can Be Modified in User Profile

**Link:** https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile

## Goal
Delete the user `carlos` by accessing the admin panel at `/admin`, which is only meant for users with `roleid: 2`.

## What's the problem?
The app lets users update their own profile (like changing their email), and this request also includes a `roleid` field. The server doesn't check whether the user is allowed to change this value — so a normal user can simply set their own `roleid` to `2` and become an admin.

## Login credentials
- **Username:** wiener
- **Password:** peter

## How to solve it (step by step)

1. Log in to the lab using `wiener` / `peter`.
2. Open **Burp Suite** and turn on request interception (Proxy tab).
3. Go to **My Account** and change your email address to trigger a request.
4. In Burp, find the captured request to `/my-account/change-email`.
5. Send this request to **Repeater**.
6. Look at the request body — it includes a field like `"roleid":1`.
7. Change this value from `1` to `2`.
8. Send the request — the server accepts it and updates your role.
9. Go back to the website and open `/admin` — the admin panel is now accessible.
10. Find the user `carlos` and delete the account.
11. Done! Lab solved.

## What we learn
Never let users control sensitive fields (like a role or permission ID) through data they send in a request. Even a routine action like updating an email should not allow changes to fields that decide access levels. The server must always decide and control role assignments — not the client.
