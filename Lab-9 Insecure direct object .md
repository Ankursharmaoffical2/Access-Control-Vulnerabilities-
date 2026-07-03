# Lab 9: Insecure Direct Object References

**Link:** https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references

## Goal
Find the password for the user `carlos` and log into his account.

## What's the problem?
The application stores live chat transcripts as plain files on the server (like `1.txt`, `2.txt`, etc.) and lets users download their own transcript using a simple, predictable URL. Since there's no check on which transcript belongs to which user, anyone can change the file number in the URL and read someone else's chat log — this is called an **Insecure Direct Object Reference (IDOR)**.

## How to solve it (step by step)

1. Open the lab website.
2. Go to **Live Chat** and start a conversation (send any message).
3. Click **View Transcript** — this downloads your chat log, e.g. as `2.txt`.
4. Open **Burp Suite** and turn on request interception (Proxy tab), or simply check the download URL directly.
5. Look at the URL used to fetch the transcript — it will look something like `/chat/2.txt`.
6. Change the file number to a lower one, e.g. `/chat/1.txt`.
7. Send the request — you get a `200 OK` response, meaning the file exists and is accessible.
8. Open/download this file and read its contents.
9. Inside, you'll find a chat log where the user reveals their password (often because they contacted support asking to reset it, and support replied with the password) — this belongs to `carlos`.
10. Copy carlos's password.
11. Go to the login page and log in as:
    - **Username:** carlos
    - **Password:** (the password found in the chat log)
12. Done! Lab solved.

## What we learn
Storing sensitive files with simple, predictable names (like `1.txt`, `2.txt`) and serving them without checking who owns them is a serious flaw. This is called an **Insecure Direct Object Reference (IDOR)** — when an app exposes internal file/object references directly to users without proper access control. Every file or resource request should be checked against the logged-in user's actual ownership, not just served based on a guessable identifier.
