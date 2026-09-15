# Evidence Exhibits

This directory documents the screenshots collected during the Wireshark investigation.

## Screenshot filenames

The following screenshots document the key evidence identified during the Wireshark investigation:

- `01-anonymous-email-http-stream.png` — Followed the HTTP/TCP stream for the anonymous-email `POST /send.php` transaction, exposing the submitted form data and server response.

- `02-gmail-account-jcoachj-search.png` — Packet-byte search for `jcoachj`, identifying the Gmail account string within captured network traffic.

- `03-threatening-message-subject-content.png` — HTTP form submission containing the threatening message subject and message content, including "you can't find us," "and you can't hide from us," "Stop teaching," and "Start running."

- `04-gmail-jcoachj-packet-filter.png` — Wireshark filtering used to isolate traffic associated with the `jcoachj@gmail.com` artifact.

- `05-jcoachj-correlation.png` — Additional packet evidence used to correlate the Gmail account artifact with network activity from the workstation.

- `06-anonymous-email-timestamp.png` — Packet frame details documenting the timestamp associated with the anonymous-email activity.

- `07-ava-book-identity-evidence.png` — Captured web content providing an additional identity-related artifact examined during the investigation.

- `08-anonymous-email-form-fields.png` — Decoded anonymous-email form fields showing the destination email, sender, subject, message, security code, and submission values.

## Evidence handling note

These exhibits come from a training packet capture and are presented for educational network-forensics analysis. Findings should distinguish direct packet evidence from attribution. An email/account string appearing in traffic does not by itself prove the identity of the person operating the host.
