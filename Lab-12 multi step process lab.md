# Lab 12: Multi-Step Process with No Access Control on One Step

**Link:** https://portswigger.net/web-security/access-control/lab-multi-step-process-with-no-access-control-on-one-step

## Goal
Log in as `wiener` and promote yourself to administrator by exploiting a flaw in a multi-step admin process.

## What's the problem?
Promoting a user to admin happens in multiple steps (e.g., Step 1: select user & choose "Upgrade" → Step 2: confirm the upgrade). The app correctly checks permissions on the *first* step, but forgets to check them again on the *final* confirmation step — which is the step that actually performs the action. This lets someone skip straight to that unprotected final step.

## Step 1: Explore as admin
1. Log in using:
   - **Username:** administrator
   - **Password:** admin
2. Open **Burp Suite** and turn on request interception (Proxy tab).
3. Go to the admin panel and start promoting a test user — click "Upgrade," then confirm on the next screen.


4. In Burp's history, find the **second request** — the one that actually confirms/executes the upgrade (this is the unprotected step). It will look something like:

POST /admin-roles
username=some-user&action=upgrade

5. Right-click this request and send it to **Repeater** (or note down its exact format).
6. Log out of the admin account.

## Step 2: Exploit as a normal user
7. Log in using:
   - **Username:** wiener
   - **Password:** peter
8. In Burp/Repeater, take the confirmation request you saved earlier.
9. Update the session cookie in that request to match your current `wiener` session (copy it from any request made while logged in as wiener).
10. Change the `username` parameter to `wiener`.
11. Send the request — since this final confirmation step never checks whether the requester is actually an admin, it goes through successfully.
12. Refresh the site — you are now an administrator.
13. Done! Lab solved.

## What we learn
When an action is split into multiple steps, access control must be enforced on **every** step — not just the first one. Attackers can skip directly to the final step (which usually performs the real action) if it's left unprotected, completely bypassing the intended flow. Every step that changes data or performs a sensitive action needs its own independent permission check.
