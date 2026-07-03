# Lab 11: Method-Based Access Control Can Be Circumvented

**Link:** https://portswigger.net/web-security/access-control/lab-method-based-access-control-can-be-circumvented

## Goal
Log in as `wiener` and promote yourself to administrator by exploiting a flaw in how access control checks the HTTP method.

## What's the problem?
The app checks access control differently depending on the HTTP method used (like `POST` vs `GET`). It properly blocks non-admins from using `POST` requests to admin actions, but forgets to apply the same restriction when the same action is sent using a different method (e.g. `GET`). This lets a normal user bypass the check just by changing the request method.

## Step 1: Explore as admin
1. Log in first using:
   - **Username:** administrator
   - **Password:** admin
2. Go to the **admin panel** and find the option to promote a user to admin.
3. Open **Burp Suite** and turn on request interception (Proxy tab).
4. Click "Upgrade" or "Promote" on a test user and capture the request — it will look like a `POST` request, e.g.:
POST /admin/upgrade
username=some-user
5. Note this request format, then log out.

## Step 2: Exploit as a normal user
6. Log in using:
   - **Username:** wiener
   - **Password:** peter
7. Try to open `/admin` — you'll be blocked, since wiener isn't an admin.
8. Go back to Burp and take the earlier captured `POST /admin/upgrade` request, send it to **Repeater**.
9. Change the HTTP method from `POST` to `GET` (Burp Repeater has a right-click option "Change request method").
10. In the request/query, set the username to `wiener`:
11. GET /admin/upgrade?username=wiener
12. 11. Send the request — since the app only checks permissions for `POST` requests, this `GET` request goes through unchecked.
12. Refresh the site — you are now an administrator.
13. Done! Lab solved.

## What we learn
Access control checks must apply consistently no matter which HTTP method is used to reach an action. If a sensitive endpoint is protected only for one method (like `POST`) but still works with another (like `GET`), attackers can bypass the entire protection just by switching methods. Authorization checks should be enforced at the endpoint/action level, not tied to a specific HTTP verb.
