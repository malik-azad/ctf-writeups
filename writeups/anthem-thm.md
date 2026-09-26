# Anthem — TryHackMe Walkthrough

> **Room:** [Anthem](https://tryhackme.com/room/anthem) · **Difficulty:** Easy · **OS:** Windows Server 2019
> **Skills:** Reconnaissance · OSINT · Page Source · NTFS Permissions
> **Tools:** Nmap · Browser · Remmina · File Explorer
>
> **Author:** [Azad Ahmad Malik](https://github.com/malik-azad) — Penetration Tester · Application Security · Red Teaming
> [GitHub](https://github.com/malik-azad) · [LinkedIn](https://www.linkedin.com/in/malikazad) · [malik-azad.github.io](https://malik-azad.github.io/)

Anthem looks like a web challenge but it's really a **Windows** challenge. There is no exploit to find here — no CVE, no payload, no shell. The whole box is solved by reading carefully and connecting four small clues. That's what makes it a genuinely good first room.

---

## TL;DR

<details>
<summary>Answers — check off as you go</summary>

**Task 1**

| Question | Answer |
|---|---|
| Open ports | `80, 3389` |
| Web server port | `80` |
| Remote desktop port | `3389` |
| Password in a crawler's page | `UmbracoIsTheBest!` |
| CMS | `Umbraco` |
| Domain | `anthem.com` |
| Administrator's name | `Solomon Grundy` |
| Administrator's email | `SG@anthem.com` |

**Task 2 — flags**

| # | Flag | Where it lives |
|---|---|---|
| 1 | `THM{L0L_WH0_US3S_M3T4}` | Page source → *We are hiring* → `og:description` meta tag |
| 2 | `THM{G!T_G00D}` | Page source → search box `placeholder` (on every page) |
| 3 | `THM{L0L_WH0_D15}` | Author page → Jane Doe (visible, not hidden) |
| 4 | `THM{AN0TH3R_M3TA}` | Page source → *A cheers to our IT department* → meta tag |

> The questions aren't in flag-number order. If one is rejected, try it in another box — only TryHackMe knows the numbering.

**Task 3**

| Question | Answer |
|---|---|
| Credentials | `sg` / `UmbracoIsTheBest!` |
| `user.txt` | `THM{N00T_NO0T}` — on SG's Desktop |
| Admin password | `ChangeMeBaby1MoreTime` — inside `C:\backup\restore.txt` |
| `root.txt` | `THM{Y0U_4R3_1337}` — on Administrator's Desktop |

</details>

---

## The whole lab in 7 commands

```bash
sudo nmap -sC -sV -A -Pn 10.49.172.250          # 1. what's open
curl -s http://10.49.172.250/robots.txt         # 2. free password
curl -s http://10.49.172.250/ | grep -o "THM{[^}]*}"   # 3. flags
xfreerdp /v:10.49.172.250 -u:sg -p:'UmbracoIsTheBest!'   # 4. log in
# on the box (PowerShell):
icacls "C:\backup\restore.txt" /grant SG:F     # 5. unlock the file
Get-Content C:\backup\restore.txt              # 6. get the admin password
xfreerdp /v:10.49.172.250 -u:administrator -p:'ChangeMeBaby1MoreTime'  # 7. root
```

Everything below is the reasoning behind those seven lines.

---

## 1. Recon — and the trap that catches everyone

Always map a machine before you touch it. Start by asking what's open:

```bash
sudo nmap -sC -sV -A -Pn 10.49.172.250
```

| Flag | Why |
|---|---|
| `-sC` | Run default scripts — one auto-fetches `robots.txt` |
| `-sV` | Detect software versions |
| `-A` | Aggressive: versions + OS guess + scripts |
| `-Pn` | **Don't ping. Go straight to probing ports.** |

### Try running it without `-Pn`

You get this:

```
Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
0 hosts up
```

The box is fine. **Windows ignores ICMP ping by default**, so nmap trusted the failed ping and declared a healthy machine dead.

```bash
ping -c 3 10.49.172.250     # 100% packet loss
```

**A failed ping is not a dead host.** On Windows, always use `-Pn`. It's the single most common beginner mistake on these targets, and it's worth memorising now.

### The results

```
80/tcp    open  http          Microsoft HTTPAPI httpd 2.0
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|   NetBIOS_Domain_Name: WIN-LU09299160F
|   Product_Version: 10.0.17763
```

- **Only two services.** No `445` (no SMB), no `5985` (no WinRM), no `22` (no SSH). Those attack paths are dead — don't waste time on them.
- `ms-wbt-server` = **Remote Desktop**. The `SERVICE` column means you never have to memorise port numbers.
- `NetBIOS_Domain_Name` equals the computer name → **not domain-joined.** Remember this for step 4; it decides your username format.

---

## 2. `robots.txt` — the file nobody reads

Before any brute-forcing, read the file the site owner wrote for search-engine crawlers:

```bash
curl -s http://10.49.172.250/robots.txt
```

```
UmbracoIsTheBest!

User-agent: *
Disallow: /bin/
Disallow: /config/
Disallow: /umbraco/
Disallow: /umbraco_client/
```

Six lines, three answers: a **plaintext password**, the **admin panel path**, and the **CMS name** — no fingerprinting tool required. The site told you what it runs.

> **Rule worth stealing:** `robots.txt` is the highest-value URL on almost any web app. Reach for a directory buster only when reading hasn't already answered the question.

---

## 3. Who is the admin? (OSINT)

The Umbraco login wants an email. We have a password, no username. The site never states one — so read the blog.

**Post 1 — "We are hiring"** gives the author's address: `jd@anthem.com`, from **J**ane **D**oe. That's a free sample of the site's email format: **first initial + last initial + @domain**.

**Post 2 — "A cheers to our IT department"** is a poem written about the admin:

> *Born on a Monday, Christened on Tuesday, Married on Wednesday,*
> *Took ill on Thursday, Grew worse on Friday, Died on Saturday,*
> *Buried on Sunday. That was the end…*

Don't guess from that — **search it.** It's the nursery rhyme **"Solomon Grundy."** That's the administrator.

Now apply the pattern you found:

| Person | Email |
|---|---|
| Jane **D**oe | `jd@anthem.com` |
| **S**olomon **G**rundy | **`SG@anthem.com`** |

Verify it at `http://10.49.172.250/umbraco/` — the page says *"your username is usually your email"*, and `SG@anthem.com` / `UmbracoIsTheBest!` gets you into the CMS.

> **Derive, never invent.** One real sample beats a hundred guesses.

---

## 4. The flags

Flags live in the **page source**, not the rendered page.

Press **`Ctrl+U`** (view source) on each page, then **`Ctrl+F`** and search `THM{`.

1. **Homepage** — in the search box's HTML:
   ```html
   <input type="text" name="term" placeholder="Search...     THM{...}" />
   ```
   It's the `placeholder` text, padded with spaces so it sits **off the visible edge of the box**. Rendered, just not *visible*.
2. **"We are hiring"** — a `og:description` meta tag near the top of the HTML.
3. **"A cheers to our IT department"** — same trick, another meta tag.
4. **Jane Doe's author page** — this one's just printed on the page, right under her name.

**Start with #4.** It shows you the flag format, and then `Ctrl+F` for `THM{` works everywhere else.

> Task 2's questions aren't in flag order. If one is rejected, try it in another box — only TryHackMe knows the numbering.

---

## 5. Getting in

```bash
xfreerdp /v:10.49.172.250 -u:sg -p:'UmbracoIsTheBest!' /cert:ignore
```

Or open **Remmina** → **"+"** → Server `10.49.172.250`, Username `sg`, Password `UmbracoIsTheBest!`, **Domain left empty**, Security `Negotiate` → Connect → accept the certificate.

### Use `sg`. Not `sg@anthem.com`.

| Situation | Format |
|---|---|
| **Standalone — this box** | **`sg`** |
| Domain-joined | `DOMAIN\user` |
| Umbraco web login | `SG@anthem.com` |

Get this wrong and the error is **identical to a wrong password** — which is why people abandon this room thinking the password is broken. Nmap told you the answer in step 1; that `NetBIOS_Domain_Name` line was the whole clue.

`user.txt` is on the Desktop.

---

## 6. Privilege escalation — the best lesson in the room

> **Privilege escalation** = going from an ordinary user to an administrator. Two routes: a bug in the software, or a mistake in how it's **configured**. This room is the second kind.

The hint says *"Can we spot the admin password?"* with the hint *"It is hidden."*

**Reveal it:** File Explorer → **View** → **Options** → tick **"Hidden files, folders, and drives"**. Back to `C:\` — a folder named **`backup`** appears. Inside is **`restore.txt`**.

Double-click it: **access denied.**

### Why — and why that's the whole attack

Right-click → **Properties** → **Security** tab. The permissions list is **completely empty**. Nobody can read this file — not Administrators, not SYSTEM, not you.

But it still has an **owner**, and the owner is **you**.

Windows splits file access into two things:

- the **DACL** — the list of who gets what permission (empty here)
- the **owner** — who *always* holds an implicit right to **change the permissions**, regardless of the DACL

So the administrator locked this file down so thoroughly that **no account could read it** — and in doing so, **made you the owner.** The "security measure" handed you the key to undo it.

That's not a vulnerability. It's a misconfiguration — and it's one of the most common real-world Windows findings there is. A file locked down properly but **owned by the wrong account** is still wide open.

> **Habit to build:** always ask *why* you have access, not just *whether* you do.

### Fix it (graphical)

Still on the **Security** tab: **Edit → Add →** type `SG` → **Check Names** → tick **Full control** → **Apply** → OK.

Now `restore.txt` opens:

```
ChangeMeBaby1MoreTime
```

That's the Administrator password.

<details>
<summary>Command-line alternative</summary>

Open PowerShell with **`Win+R` → `powershell`** (always present, even on desktops with no shortcuts):

```powershell
icacls "C:\backup\restore.txt" /grant SG:F
Get-Content C:\backup\restore.txt
```

`icacls` is the terminal version of that Security tab; `/grant SG:F` = give user **S**G **F**ull control.

**Two notes.** You can skip the "show hidden files" step entirely — just type the path into Explorer's address bar. Hidden only hides from *view*; it doesn't restrict *access*. And PowerShell's `Set-Acl` will **fail** on this file, because it looks for an explicit *Change permissions* entry and the DACL is empty — `icacls` uses the owner's implicit right and works. That difference will cost you 20 minutes of confusion on a harder box.

</details>

---

## 7. Root

```bash
xfreerdp /v:10.49.172.250 -u:administrator -p:'ChangeMeBaby1MoreTime' /cert:ignore
```

Different wallpaper, different icons — you're a different user now. **`root.txt` is on the Desktop.**

---

## Takeaways

1. **`-Pn` or nothing.** Windows ignores ping. A failed ping is not a dead host.
2. **`robots.txt` first, always.** It's written *for* you. Tools fill gaps; they don't replace reading.
3. **Derive, don't guess.** One real sample (`jd@anthem.com`) gave the format for `SG@anthem.com`.
4. **OSINT is real enumeration.** A nursery rhyme found the admin.
5. **Username format depends on domain membership.** `sg` vs `DOMAIN\sg` — and it fails identically to a wrong password.
6. **Ownership is a security boundary that often isn't one.** The owner can always change permissions, even with an empty DACL. Over-restricting a file *created* this hole.
7. **Hidden ≠ protected.** That folder was reachable the whole time.
8. **No exploit was required.** Six flags, zero vulnerabilities. Real testing is mostly reading, not running.

---

## Author

**Azad Ahmad Malik** — Penetration Tester | Application Security | Red Teaming

| | |
|---|---|
| **GitHub** | [@malik-azad](https://github.com/malik-azad) |
| **LinkedIn** | [in/malikazad](https://www.linkedin.com/in/malikazad) |
| **Website** | [malik-azad.github.io](https://malik-azad.github.io/) |

Cybersecurity & Networking Enthusiast · Application Security · Penetration Tester · Python, Django, Linux · Red Teaming

---

*Educational walkthrough of a [TryHackMe](https://tryhackme.com) lab. The techniques covered are standard offensive-security fundamentals — always practise only on systems you own or are explicitly authorised to test.*

