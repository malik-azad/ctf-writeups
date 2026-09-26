# CTF Writeups

Walkthroughs of CTF rooms and lab machines, written to be **reproducible**,
not just readable. Every writeup explains why a step was chosen, not only what
to type.

These are labs built for security education: TryHackMe, HackTheBox, and
similar deliberately vulnerable environments. All of them verified on the
target, with the flag values included so you can check your own work.

## Writeups

| # | Lab | Platform | Difficulty | Focus | Writeup |
|---|-----|----------|-----------|-------|---------|
| 1 | **Anthem** | TryHackMe | Easy | Reconnaissance, OSINT, page source, NTFS permissions | [anthem-thm.md](writeups/anthem-thm.md) |

## What each writeup contains

A single self-contained Markdown file, structured the same way every time:

- **TL;DR** — what the room was actually about, in a couple of lines
- **A spoiler answer key** — every task answered with the real flag value, collapsed so it does not spoil the read
- **The walkthrough** — reconnaissance, exploitation, and post-exploitation in the order they actually happened
- **What each finding means** — the concept behind the step, not just the command
- **Dead ends** — the things that did not work, so you do not repeat them
- **Screenshots to take** — a checklist, if you are documenting your own attempt

That last part matters more than it sounds. A writeup that hides the answer
key is a walkthrough. A writeup that explains *why* the page source contained
the flag teaches you to look at page source on the next twenty machines.

---

## Approach

A few principles run through every writeup here:

- **Map before you attack.** Understand the surface before choosing a technique.
- **Enumerate or lose.** Most of these rooms are solved by something that was sitting in plain sight during enumeration.
- **Cheapest useful move first.** Escalate effort only when the evidence says you must.
- **Read output for decisions, not just data.** Every line should either open an option or close one.
- **Verify before you trust.** Reproduce it on the target. Do not inherit an answer key from someone else's writeup.
- **Document as you go.** Notes, exact locations, and dead ends are what make a report possible later.

---

## Related

The general-purpose field notes, cheatsheets, and checklists I use across
pentest engagements are in a separate repository, so this one stays focused on
walkthroughs:

**[pentest-field-notes](https://github.com/malik-azad/pentest-field-notes)** —
recon, web and API testing, privilege escalation, Active Directory, cloud,
containers, pivoting, password attacks, mobile, and low-noise testing.

---

## Author

**Azad Ahmad Malik** — Penetration Tester, Application Security, Red Teaming

| | |
|---|---|
| **GitHub** | [@malik-azad](https://github.com/malik-azad) |
| **LinkedIn** | [in/malikazad](https://www.linkedin.com/in/malikazad) |
| **Website** | [malik-azad.github.io](https://malik-azad.github.io/) |

Cybersecurity and networking enthusiast. Application security, penetration
testing, and red teaming. Python, Django, and Linux. Currently working through
OSCP and CPENT material.

---

## Scope and ethics

Every room documented here is a **publicly available CTF room or a
deliberately vulnerable lab environment** built for security education. They
exist so people can learn offensive security fundamentals safely and legally.

**Test only systems you own or have explicit written authorisation to test.**
Written authorisation, for the specific target and the specific test, agreed
before the first packet is sent. A host being reachable does not make it yours,
and a scope document agreed in the morning may have changed by the afternoon.

If you find a vulnerability in a system you are not authorised to test, report
it to the owner through their published disclosure process and stop there.
