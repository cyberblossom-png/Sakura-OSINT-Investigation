# TryHackMe — Sakura OSINT Investigation

> A hands-on passive OSINT investigation completed in Kali Linux while working through the TryHackMe Sakura room.

## Objective

The objective of this investigation was to follow publicly available clues and identify information associated with the attacker persona used in the Sakura OSINT challenge.

The main lesson was not simply finding an answer, but learning how to **pivot from one clue to another and verify connections across multiple sources**.

## Tools Used

- Kali Linux
- ExifTool
- GPG
- Google
- GitHub
- X (Twitter)

## Investigation Chain

**Image metadata → username → GitHub → PGP key → User ID/email clue → X account → identity clue**

---

## 1. Extracting Metadata

The investigation started with the supplied `sakurapwnedletter.svg` file.

I used ExifTool to inspect the file metadata:

```bash
exiftool sakurapwnedletter.svg
```

One useful field was the exported filename/path, which exposed the username used in the challenge.

The username became my first OSINT pivot.

![ExifTool metadata](screenshots/04-exiftool.jpeg)

---

## 2. Pivoting to GitHub

I searched the username and found a matching GitHub profile.

The profile contained several public repositories, including a repository named **PGP**.

This was an important pivot because the username alone did not reveal all the information needed for the investigation.

![GitHub profile](screenshots/05-github-profile.jpeg)

---

## 3. Investigating the PGP Repository

The PGP repository contained a public key file named `publickey`.

I downloaded the file for local analysis.

For the public portfolio, the contents of the key have been redacted.

![PGP public key](screenshots/02-pgp-redacted.png)

---

## 4. Examining the PGP Key with GPG

I used GPG to inspect the public key:

```bash
gpg --show-keys publickey
```

The output included a `uid` (User ID) field.

In OpenPGP keys, the User ID can contain identifying information such as a name or email address. In this investigation, that field provided the next clue.

Sensitive contact information and the full key fingerprint are redacted in the public screenshot.

![GPG output](screenshots/01-gpg-redacted.png)

### What I learned

- `gpg` can inspect OpenPGP public keys.
- The `uid` field can contain useful identity metadata.
- Cryptographic artifacts can sometimes provide OSINT clues in addition to their security purpose.

---

## 5. Pivoting to X (Twitter)

I returned to the username and searched for associated accounts.

Google surfaced an X account using the persona `@SakuraLoverAiko`.

A post from that account provided another handle, which became the next pivot in the investigation.

![Google search results](screenshots/06-google-search.jpeg)

---

## 6. Establishing the Identity Clue

The X account contained a post introducing another handle.

Rather than relying on a single search result, I correlated:

1. The original username
2. The GitHub account
3. The PGP repository
4. The PGP User ID clue
5. The X account
6. The secondary X handle

This allowed me to establish the identity clue required by the TryHackMe task.

---

## Key Takeaways

### 1. Metadata matters

Files can contain information that is not visible when simply opening the file.

### 2. Usernames are powerful pivot points

A username found in one source can lead to related accounts on other platforms.

### 3. PGP keys can contain identity metadata

The cryptographic key itself was not the final answer; the User ID metadata became the useful OSINT clue.

### 4. OSINT is about correlation

The strongest part of this investigation was connecting several small clues rather than trusting one result.

### 5. Verify before concluding

Search-engine results and third-party sites can contain false positives. I learned to distinguish between:

- an original source,
- a secondary write-up,
- and an unrelated person with a similar name.

## Skills Practiced

- Passive OSINT
- Metadata analysis
- ExifTool
- OpenPGP/GPG inspection
- Username enumeration through public sources
- Cross-platform pivoting
- Evidence correlation
- Basic OPSEC/privacy awareness

## Note

This write-up documents a fictional/CTF investigation performed in a controlled TryHackMe training environment. Sensitive contact information and cryptographic material have been redacted from public screenshots.

---

**Lab:** TryHackMe — Sakura  
**Platform:** Kali Linux  
**Focus:** Passive OSINT
