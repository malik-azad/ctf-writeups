# Cheatsheets

Field notes I keep open in a second window. Not a course, not a certification
dump. Just the commands, order of operations, and traps I had to learn the hard
way, written down so I stop relearning them.

## How to use these

Read them in order the first time. After that they are reference material.

The ordering matters more than it looks. Almost every wasted week in this field
comes from skipping recon and going straight to exploitation. The sequence in
[recon.md](recon.md) is the sequence that works.

| Sheet | What it covers |
|---|---|
| [recon.md](recon.md) | Host discovery, nmap, service enumeration, wordlists |
| [web-testing.md](web-testing.md) | Web app assessment, common bug classes, JS analysis |
| [api-testing.md](api-testing.md) | REST and GraphQL testing, auth, BOLA/IDOR |
| [bug-bounty.md](bug-bounty.md) | Scope, recon at scale, triage, reporting |
| [privilege-escalation.md](privilege-escalation.md) | Linux and Windows privesc, ordered |
| [evasion-and-bypass.md](evasion-and-bypass.md) | Low-noise testing, WAF and filter bypass, tunnels |
| [checklists.md](checklists.md) | Point-by-point lists for each assessment phase |
| [field-notes.md](field-notes.md) | Scope discipline, note-taking, evidence, what to do when stuck |

## One thing before anything else

None of this works on systems you are not authorised to test. Not your own
machine, not a lab you have not signed up for, not "it is only a school
project". Written authorisation, every single time, including for the people who
persuade you it would be fine.

Two things make this non-negotiable for a professional:

1. Get scope in writing before you send a single packet, and re-read it during
   the engagement. Scope gets amended mid-test more often than people expect.
2. Know the difference between "in scope" and "technically possible". A host
   being reachable does not make it yours. A subdomain being on the same
   registrar does not make it yours. Shared hosting and CDNs put other
   customers' systems on the same IP.

Details on how to handle this properly are in
[field-notes.md](field-notes.md), because it is the part that decides whether
you are a pentester or a liability.

## Beginner to advanced

Where you are determines which sheets matter most.

**Just starting.** Read [recon.md](recon.md) and [checklists.md](checklists.md)
end to end, then do a TryHackMe or HackTheBox machine without peeking. The habit
of finishing a full methodology pass matters more than any single technique.

**Comfortable with labs, want a job.** [web-testing.md](web-testing.md) and
[api-testing.md](api-testing.md) are where the interview questions and the
real bug bounty findings live. [bug-bounty.md](bug-bounty.md) for the business
side, which nobody teaches you and which decides whether you get paid.

**Doing engagements.** [privilege-escalation.md](privilege-escalation.md) and
[evasion-and-bypass.md](evasion-and-bypass.md) carry the weight. Client
environments have EDR, WAFs, and change windows, and you will need to work
inside them rather than around them noisily.

**Preparing for OSCP or CPENT.** Both are practical. The exam machines reward
exactly what [checklists.md](checklists.md) lays out, executed in order, without
the machine fighting you. The AD material in the CPENT path is worth studying
separately.

## A note on tools

Tools change. Their names change, their flags change, half of them get
superseded every eighteen months. The order of operations does not change.

Learn to do the core work by hand first. nmap, curl, openssl, ssh, and a text
editor cover most of what you need on a real engagement, and a tool you do not
understand is a tool you cannot troubleshoot when it fails at 2am. Automation
comes after, as an accelerant, not a replacement.

Every command in these sheets has a plain explanation next to it. If you copy
something without reading the line under it, you have not learned it yet.

## Contributing

Corrections welcome, especially the ones of the form "this flag does not work
on current versions". If something here is wrong or outdated, open a pull
request and say which version you tested on.
