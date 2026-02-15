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

I tested my luck manually tried common ones like:

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

At first, I ran Gobuster against the root directory, but it didn’t reveal anything useful.

So instead, I focused enumeration specifically on the suspicious directory:

- `/cupids_secret_vault`

This is an important habit in CTFs:

> If the root is clean, enumerate deeper paths.

---

## Solution (What Worked)

### Key Idea

The challenge relied on **basic web enumeration** and **credential reuse** from `robots.txt`.

### Steps

1. Visit the target website in the browser  
2. Navigate to:

