# Mail Message Format: RFC 5322 Standards

This document explains the internal structure of an e-mail message, distinguishing it from the SMTP transport protocol.

## 1. The Structure of an E-mail
An e-mail message is composed of three distinct parts, defined by the RFC 5322 standard. This structure is essential for interoperability between different mail clients (e.g., Gmail to Outlook).

1.  **Header Lines:** Metadata about the message (e.g., Sender, Recipient, Date, Subject).
2.  **Blank Line:** A mandatory empty line (CRLF) that acts as the "separator" between the headers and the body.
3.  **Message Body:** The actual content of the message (in ASCII format).

## 2. Key Concept: SMTP Commands vs. Message Headers
It is a common mistake to confuse these two layers. They are functionally different:

* **SMTP Commands (Transport Layer):**
    * Example: `MAIL FROM:`, `RCPT TO:`.
    * **Purpose:** These are instructions for the mail server to route the message. The server reads these to know where to send the mail.
* **Message Headers (Application Layer):**
    * Example: `From:`, `To:`, `Subject:`.
    * **Purpose:** These are part of the message itself. The server does not act on these; it simply delivers them as data to the final recipient.

**Why this matters:** A user can send a message using an SMTP command, but the header inside the message might contain completely different information. This separation is why the "Blank Line" separator is critical for the server to stop parsing commands and start reading data.

## 3. Practical Example
A typical message format looks like this:

```text
From: alice@crepes.fr
To: bob@hamburger.edu
Subject: Searching for the meaning of life

[This is the blank line separating header from body]

Hi Bob, I have been thinking about the meaning of life...
```

## 4. Engineering Insights
* The Parsing Rule: The mail client reads headers until it hits the Blank Line. Once it hits that line, it stops looking for keywords (like "Subject:") and treats everything else as the message body.

* Standardization: Because of RFC 5322, every mail agent in the world knows exactly how to parse this format, regardless of which operating system or application is being used.

## Note:
Engineering Note: Understanding this separation is the foundation for advanced features like e-mail attachments, where a file is encoded as a block of text and placed within the message body, following these same structural rules.
