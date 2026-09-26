# Writeups and Field Notes

Walkthroughs from CTF platforms and penetration testing practice, plus the
cheatsheets I keep open in a second window while testing.

The goal is reproducible, not just readable. Every writeup explains why a step
was chosen, not only what to type.

## Contents

- [Writeups](#writeups)
- [Cheatsheets](#cheatsheets)
- [Approach](#approach)
- [Author](#author)
- [Scope and ethics](#scope-and-ethics)

---

## Writeups

| # | Lab | Platform | Difficulty | Focus | Writeup |
|---|-----|----------|-----------|-------|---------|
| 1 | **Anthem** | TryHackMe | Easy | Reconnaissance, OSINT, page source, NTFS permissions | [anthem-thm.md](writeups/anthem-thm.md) |

Each writeup is a single self-contained file: a spoiler answer key, the
reconnaissance behind it, what each finding actually means, and the dead ends
worth knowing about so you do not repeat them.

---

## Cheatsheets

Notes to self that turned into something worth sharing. Written in a plain
register and meant to be read once through in order, then used as reference.

| Sheet | Covers |
|---|---|
| [recon.md](cheatsheets/recon.md) | Host discovery, nmap, per-service enumeration, wordlist locations |
| [web-testing.md](cheatsheets/web-testing.md) | Web app assessment, the bug classes that pay, JS analysis |
| [api-testing.md](cheatsheets/api-testing.md) | REST and GraphQL, authentication, BOLA and IDOR, mass assignment |
| [bug-bounty.md](cheatsheets/bug-bounty.md) | Scope, recon at scale, triage, deduplication, reporting |
| [privilege-escalation.md](cheatsheets/privilege-escalation.md) | Linux and Windows, in enumeration order, plus AD |
| [evasion-and-bypass.md](cheatsheets/evasion-and-bypass.md) | Low-noise testing, WAF and filter bypass, tunnels, EDR awareness |
| [checklists.md](cheatsheets/checklists.md) | Tick-box lists for every phase, recon through exam day |
| [field-notes.md](cheatsheets/field-notes.md) | Scope discipline, note-taking, evidence, what to do when stuck |

Start with [field-notes.md](cheatsheets/field-notes.md) if you have not tested
before. It covers the parts of the job that are not technical, and they are the
parts that decide whether you are still working in three years.

Two notes on how to read the set:

- **Order matters.** Almost every wasted week in this field comes from skipping
  recon and going straight to exploitation. The sequence in the sheets is the
  sequence that works.
- **Tools change, order does not.** Every command has a plain explanation beside
  it. Learn to do the core work by hand first, and treat automation as an
  accelerant rather than a replacement.

---

## Approach

A few principles run through everything here:

- **Map before you attack.** Understand the surface before choosing a technique.
- **Enumerate or lose.** Most findings, in labs and on client networks alike, come from something that was sitting in plain sight during enumeration.
- **Cheapest useful move first.** Escalate effort only when the evidence says you must.
- **Read output for decisions, not just data.** Every line should either open an option or close one.
- **Verify before you trust.** Reproduce it, cross-check it with a second method, and run a benign control. Suspected is not confirmed.
- **Prove, then stop.** Demonstrate the flaw, record the evidence, and leave the data alone. Access to something does not entitle you to read it.
- **Document as you go.** Notes, exact locations, and dead ends are what make a report possible later.

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

Corrections are welcome, especially the "this flag no longer works on current
versions" kind. Open a pull request and say which version you tested on.

---

## Scope and ethics

Everything in this repository documents **publicly available CTF rooms and
deliberately vulnerable lab environments** built for security education. They
exist so that people can learn offensive security fundamentals safely and
legally.

The material is standard professional security practice: reconnaissance,
service enumeration, vulnerability analysis, and privilege escalation review.

**Test only systems you own or have explicit written authorisation to test.**
Written authorisation, for the specific target and the specific test, agreed
before the first packet is sent. A host being reachable does not make it yours,
and a scope document agreed in the morning may have changed by the afternoon.

The techniques in `evasion-and-bypass.md` exist so that you can test whether a
control actually holds. They are not for reaching systems you have not been
authorised to reach, and the section on operational safety in that file is the
part worth reading twice.

If you find a vulnerability in a system you are not authorised to test, report
it to the owner through their published disclosure process and stop there.
