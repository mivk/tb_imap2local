# tb_imap2local
**tb_imap2local** — Copy Thunderbird IMAP account folders to Local Folders

SYNOPSIS
========
**tb_imap2local** \[source] \[destination]

DESCRIPTION
===========
Bash script to copy mailboxes and folders from an IMAP account's local cache in Thunderbird to a folder in "Local Folders".

Thunderbird lets you copy or move messages, but not folders or (mutliple) mailboxes.

If you simply copy the mbox files
*   from the local cache of an IMAP account (files under `YourProfile/ImapMail/your.hostname.tld`),
*  to your Local Folders directory (`YourProfile/Mail/Local Folders/`)

it will not work.

The main problem is related to the mbox format used by default in Thunderbird. It uses lines which begin with "`From `" to separate messages. So Thunderbird (and other mail programs using this format) need to escape such lines in emails when writing to the mbox file, and then unescape them when showing the mail in the interface. So a line starting with eg. "From now on, ..." in an email is written as ">From now on, ..." in the file.

**BUT**, when Thunderbird is saving local copies of messages from an IMAP account, it does not escape such lines. So if such a file is copied to Local Folders, any messages which contain lines starting with "From " will be split into separate messages, corrupting the entire mailbox.

This script escapes the "From " lines after copying the maibox file.

It also creates empty mailbox where where needed. Without these, folders with subfolders or mailboxes, but without messages in their root folder would not be shown.

# Usage

* Make sure you configured your IMAP account to download all mails
* Briefly go over each mailbox in each folder or subfolder to force TB to actually do the download.
* Close Thuderbird
* Run the script with the source and destination folders as arguments.
* Remove the `.bak` backup files created while updating the mailboxes: use them to check the changes made if you like, then use the commands shown after the script run to delete the backup files.

# Example
    tb_imap2local "$HOME/.thunderbird/xx.default/ImapMail/mail.example.com" \
    "$HOME/.thunderbird/xx.default/Mail/Local Folders/my_old_example_mails"


SEE ALSO
========

