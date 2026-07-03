# Lab 1: Unprotected Admin Functionality

**Link:** https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality

## Goal
Delete the user `carlos` by finding the hidden admin page.

## What's the problem?
The admin page is not really secured. It's just "hidden" — there's no login check. If you find the link, you can open it and use it like an admin.

## How to solve it (step by step)

1. Open the lab website.
2. Go to `robots.txt` (add `/robots.txt` at the end of the URL).
3. Look inside it — it will show a page the developers didn't want search engines to find, something like `/administrator-panel`.
4. Copy that page name and open it in your browser.
5. Since there's no protection, the admin panel opens right away.
6. Find the user `carlos` and click delete.
7. Done! Lab solved.

## What we learn
Hiding a page is not the same as protecting it. Even if a link isn't shown anywhere, anyone who finds it can still use it — unless the page checks who is logging in. Real security means checking permissions, not just hiding the URL.
