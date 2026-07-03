# Lab 6: User ID Controlled by Request Parameter, with Unpredictable User IDs

**Link:** https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids

## Goal
Find carlos's GUID, then use it to get his API key and submit it as the solution.

## What's the problem?
This lab is similar to the last one — a horizontal privilege escalation on the account page — but instead of a simple username, users are identified by random GUIDs (long unique IDs). The catch is that these GUIDs aren't fully secret; they get exposed elsewhere in the app, like in blog post authorship links.

## Login credentials
- **Username:** wiener
- **Password:** peter

## How to solve it (step by step)

1. Log in to the lab using `wiener` / `peter`.
2. Go to **My Account** — note that your account URL now contains a GUID instead of your username, e.g. `/my-account?id=<your-guid>`.
3. Go to the blog section and open any post written by `carlos`.
4. Click on carlos's name/author link on the post — this reveals his GUID in the URL.
5. Copy carlos's GUID.
6. Open **Burp Suite** and turn on request interception (Proxy tab).
7. Reload your own account page so Burp captures the request (e.g. `GET /my-account?id=<your-guid>`).
8. Send this request to **Repeater** (or intercept and forward it directly).
9. Replace your GUID in the request with carlos's GUID.
10. Send/forward the request — the response now shows carlos's account page, including his API key.
11. Copy carlos's API key and submit it as the solution.

## What we learn
Using random GUIDs instead of simple usernames doesn't fix the vulnerability — it just makes the ID harder to guess. If the ID still gets exposed somewhere else in the app (like an author link on a post), an attacker can find it and use it the same way. Real protection means checking that the logged-in user actually owns the resource they're requesting, regardless of how unpredictable the ID looks.
