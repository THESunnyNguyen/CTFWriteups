# CTF Writeup — Hidden Deep into My Heart

**Challenge:** Hidden Deep into My Heart  
**Event:** Love at First Breach 2026 (TryHackMe)  
**Category:** Web  
**Challenge Author:** TryHackMe, munra, kohzmik

**Writeup Author:** Sunny Nguyen  
**Tools Used:** Browser tools, robots.txt, Gobuster  
**Difficulty:** Easy  

---

## TL;DR

I checked `robots.txt`, found a hidden directory and a password clue, then used Gobuster to enumerate deeper paths. The `/administrator` page accepted `admin` + the password from `robots.txt`, revealing the flag.

---

## Challenge Description

<img width="728" height="280" alt="image" src="https://github.com/user-attachments/assets/3753e815-184e-404f-b7f8-185066caaf69" />

---

## Initial Analysis

When I first started the challenge, I immediately checked for common CTF web indicators. The first thing I looked at was:
- `robots.txt`

For anyone unfamiliar:  
`robots.txt` is a publicly accessible file for websites

And there was something interesting inside.

<img width="573" height="189" alt="image" src="https://github.com/user-attachments/assets/db62bc28-12c4-428a-a2a9-e10e5432461f" />


---

## Constraints / Obstacles

The challenge was designed to hide the flag behind:

- A non-obvious hidden directory
- A deeper subdirectory not visible from the homepage
- A login page with weak credentials (but not immediately visible)

---

## First Attempt 

My first instinct was:

> Navigate directly to the hidden path listed in `robots.txt`

That got me to a page that looked empty/useless at first… but it included a hint:

<img width="604" height="509" alt="image" src="https://github.com/user-attachments/assets/0fa63ca4-a941-4902-b148-3c34fb79fad3" />


> “There’s more to discover”

So I checked the page source and explored all of the possible tabs in the web developer tools by inspecting the page. After hitting a dead end, I decided to enumerate possible directories.

I tested my luck manually tried possible ones like:

- /cupids_secret_vault/cupid_arrow_2026!!!
- /cupids_secret_vault/flag.txt
- /cupids_secret_vault/flag
- /cupids_secret_vault/login

<img width="927" height="233" alt="image" src="https://github.com/user-attachments/assets/2dc4b7df-de7c-455d-86e8-dcc5d58a5469" />

After hitting my head against the wall, I decided to use Gobuster to speed up the process.

---

## Investigation

### What I Checked

- `robots.txt`
- Manual navigation to the hidden directory
- Directory enumeration using Gobuster

### Notes

At first, I ran Gobuster against the root directory, but it didn’t reveal anything useful since we've already checked robots.txt and /console didn't output anything.

<img width="1070" height="380" alt="image" src="https://github.com/user-attachments/assets/39467466-b2f0-4936-ac23-0ba707290fc2" />

<img width="630" height="178" alt="image" src="https://github.com/user-attachments/assets/fd67e023-dc2f-484d-bd08-bc3ed63c3b25" />


So instead, I focused enumeration specifically on the suspicious directory:

- `/cupids_secret_vault`

<img width="1253" height="342" alt="image" src="https://github.com/user-attachments/assets/a5b2a686-0525-4612-bc9c-98fe0c482f37" />

Then I found a /administrator page with login fields. 

<img width="639" height="413" alt="image" src="https://github.com/user-attachments/assets/8b8c9bc8-6107-4a8c-8337-175d03e01c02" />

---

## Solution (What Worked)

### Key Idea

The challenge relied on **basic web enumeration** and **credential reuse** from `robots.txt`.

### Steps

1. Visit the target website in the browser  
2. Navigate to: /robots.txt
3. Identify the hidden directory and the password hint: /cupids_secret_vault and cupid_arrow_2026!!!
4. Visit: /cupids_secret_vault
5. Notice the message hinting that more content exists  
6. Run Gobuster on the hidden directory: gobuster dir -u <TARGET>/cupids_secret_vault -w <WORDLIST>
7. Find the suspicious subdirectory: /administrator
8. Visit: /cupids_secret_vault/administrator
9. Try logical default credentials:
- Username: `administrator` (failed)
- Username: `admin` (worked)
- Password: `cupid_arrow_2026!!!`

## Flag / Result

<img width="907" height="518" alt="image" src="https://github.com/user-attachments/assets/ce4dbfac-b195-4db6-a287-8093187316e1" />

## What I Learned
- `robots.txt` is one of the first files you should check in beginner/intermediate web CTFs.
- If a page hints that “there’s more,” it often means **hidden directories**.
- Gobuster is extremely effective when you aim it at the *right* directory.
- You don’t always need brute-force tools — sometimes simple logic works faster.

## Defensive Notes (Real-World Takeaway)
### ❌ What NOT to do
- Putting sensitive paths in `robots.txt`
- Using weak admin credentials
- Reusing passwords in obvious locations

### ✅ What should be done instead
- Never store secrets in publicly accessible files
- Enforce strong password policies
- Use proper access control and authentication hardening
- Monitor for directory enumeration and brute-force attempts
