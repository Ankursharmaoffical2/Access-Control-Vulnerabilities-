# Lab 2: Unprotected Admin Functionality with Unpredictable URL

**Link:** https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url

## Goal
Delete the user `carlos` by finding and accessing the hidden admin panel.

## What's the problem?
This time the admin panel isn't at a simple, guessable URL like `/admin` or `/administrator-panel`. Its real location is random — but the link to it is still leaked somewhere inside the app's front-end code.

## How to solve it (step by step)

1. Open the lab website.
2. Try guessing common admin URLs like `/admin` or `/admin-panel` — they won't work, since the real path is random.
3. Open the browser's **Inspect / Developer Tools** (right-click → Inspect).
4. Search the page source for the word `admin`.
5. You'll find a hidden link in the HTML/JavaScript, something like `/admin-page393`.
6. Copy that link and paste it into the browser's address bar.
7. The admin panel opens without any login check.
8. Find the user `carlos` and delete the account.
9. Done! Lab solved.

## What we learn
Making a URL hard to guess (a "random" or "secret" link) is still not real security. If that link is exposed anywhere in the page's source code, anyone inspecting it can find and use it. Sensitive pages must always check who's logged in — not just rely on a hidden or unpredictable address.
