# Evidence Exhibits

This directory documents the screenshots collected during the Wireshark investigation.

## Screenshot filenames

Place the corresponding screenshots in this directory using these names:

- `01-anonymous-email-http-stream.png` — Followed HTTP/TCP stream for the `POST /send.php` transaction. The request visibly contains the anonymous-email form fields and the server returns `HTTP/1.1 200 OK`.
- `02-gmail-account-jcoachj-search.png` — Wireshark packet-byte search for `jcoachj`, showing the account string in Gmail-related traffic.
- `03-threatening-message-subject-content.png` — URL-encoded form submission displaying subject `you can't find us` and message content including `and you can't hide from us`, `Stop teaching`, and `Start running`.
- `04-gmail-jcoachj-packet-filter.png` — Traffic from `192.168.15.4` containing `jcoachj@gmail.com`, used to correlate the Gmail artifact with the workstation of interest.
- `05-anonymous-email-timestamp.png` — Frame details for packet 80614 showing arrival time July 22, 2008 at 02:02:57.548149 EDT (06:02:57.548149 UTC).

## Additional useful exhibit

A close-up of the decoded HTML form from packet 80614 can be retained as an additional exhibit because it clearly displays the destination email, sender, subject, message, security code, and submit value.

## Evidence handling note

These exhibits come from a training packet capture and are presented for educational network-forensics analysis. Findings should distinguish direct packet evidence from attribution. An email/account string appearing in traffic does not by itself prove the identity of the person operating the host.
