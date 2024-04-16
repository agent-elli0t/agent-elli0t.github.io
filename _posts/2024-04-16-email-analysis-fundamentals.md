---
title: Email Analysis Fundamentals
date: 2024-04-14
categories: [BlueTeaming]
tags: [TryHackMe]
---

# Email Analysis Fundamentals

## Learn all the components that make up an email.

It's only appropriate to start this room by mentioning the man who invented the concept of emails and made the @ symbol famous. The person responsible for the contribution to the way we communicate was Ray Tomlinson.

The invention of the email dates back to the 1970s for ARPANET. Yep, probably before you were born. Definitely, before I was born. :)

So, what makes up an email address?

- User Mailbox (or Username)
- @
- Domain

Let's look at the following email address, billy@johndoe.com.

- The user mailbox is billy
- @ (thanks Ray)
- The domain is johndoe.com

To simplify this even further, think about the street on which you live on.

- You can think of your street as the domain.
- The recipient's first/last name, along with the house number in this scenario, represents the user mailbox.
- With this information, the postal worker delivering the mail knows into which mailbox to put the letter(s).

A handful of protocols are involved with the "magic" that takes place when you hit SEND in an email client.

By now, you should already know that certain protocols were created to handle specific network-related tasks, such as email.

There are 3 specific protocols involved to facilitate the outgoing and incoming email messages, and they are briefly listed below:

- SMTP (Simple Mail Transfer Protocol) - It is utilized to handle the sending of emails.
- POP3 (Post Office Protocol) - Is responsible transferring email between a client and a mail server.
- IMAP (Internet Message Access Protocol) - Is responsible transferring email between a client and a mail server.

You should have noticed that both POP3 and IMAP have the same definition. But there are differences between the two.

### POP3

- Emails are downloaded and stored on a single device.
- Sent messages are stored on the single device from which the email was sent.
- Emails can only be accessed from the single device the emails were downloaded to.
- If you want to keep messages on the server, make sure the setting "Keep email on server" is enabled, or all messages are deleted from the server once downloaded to the single device's app or software.

### IMAP

- Emails are stored on the server and can be downloaded to multiple devices.
- Sent messages are stored on the server.
- Messages can be synced and accessed across multiple devices.

Now let's talk about how email travels from the sender to the recipient.

To best illustrate this, see the oversimplified image below:

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/9fb354e3-ffa9-4267-8023-c57384ef25bc)


Below is an explanation of each numbered point from the above diagram:

1. Alexa composes an email to Billy (billy@johndoe.com) in her favorite email client. After she's done, she hits the send button.
2. The SMTP server needs to determine where to send Alexa's email. It queries DNS for information associated with johndoe.com.
3. The DNS server obtains the information johndoe.com and sends that information to the SMTP server.
4. The SMTP server sends Alexa's email across the Internet to Billy's mailbox at johndoe.com.
5. In this stage, Alexa's email passes through various SMTP servers and is finally relayed to the destination SMTP server.
6. Alexa's email finally reached the destination SMTP server.
7. Alexa's email is forwarded and is now sitting in the local POP3/IMAP server waiting for Billy.
8. Billy logs into his email client, which queries the local POP3/IMAP server for new emails in his mailbox.
9. Alexa's email is copied (IMAP) or downloaded (POP3) to Billy's email client.

Lastly, each protocol has its associated default ports and recommended ports. For example, SMTP is port 25.

[Write all ports here]

This understanding is necessary if you wish to analyze potentially malicious emails manually.

Before we begin, we need to understand that there are two parts to an email:

- the email header (information about the email, such as the email servers that relayed the email)
- the email body (text and/or HTML formatted text)

The syntax for email messages is known as the Internet Message Format (IMF).

Let's look at email headers first.

### What do you look for when analyzing a potentially malicious email?

Let's start with the following email header fields:

- From - the sender's email address
- Subject - the email's subject line
- Date - the date when the email was sent
- To - the recipient's email address

This is usually clearly visible in any email client. Let's look at an example of these fields in the below image.

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/6e4cec22-65ce-48c9-b10a-be271657615a)


Another method to obtain the same email header information, and more, is by viewing the 'raw' email details.

When looking at an email header in detail, it can be intimidating at first, but it's not so bad if you know what to look for.

Review this Knowledge Base (KB) article from Media Temple on viewing the raw/full email headers in various email clients here.

In the below image, you can see how to view this information within Yahoo.

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/d44bf693-a46b-45f8-b64f-67e4404d7dcf)

Below is a snippet of the raw message for the email sample.

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/86d54eb8-5cc0-47e2-a236-287123caa959)

You can review this email in the Email Samples directory on the Desktop within the attached virtual machine. The email is titled email1.eml.

From the above image, there are other email header fields of interest.

- X-Originating-IP - The IP address of the email was sent from (this is known as an X-header)
- Smtp.mailfrom/header.from - The domain the email was sent from (these headers are within Authentication-Results)
- Reply-To - This is the email address a reply email will be sent to instead of the From email address

To clarify, in the email in the sample above, the Sender is newsletters@ant.anki-tech.com, but if a recipient replies to the email, the response will go to reply@ant.anki-tech.com, which is the Reply-To, and NOT to newsletters@ant.anki-tech.com.

### Understanding An Email Header
Applies to: All Service Types
Difficulty: Easy
Time Needed: 10
Tools Required: None
Last modified: March 9, 2020

**Introduction**
This guide is provided to learn how to read and understand an email header. To understand an email header, we need to analyze the life of the email. Most of the time, it appears that email is passed directly from the sender directly to the recipient. This isn't necessarily true: A typical email passes through at least four computers.

**How to view an email subheader**
In this example, the "Sender" mt.kb.user@gmail.com wants to send an email to the "Receiver" user@example.com. The sender composes the email at gmail.com, and user@example.com receives it in the email client Apple Mail.

Here is the example header:

```html
From: Media Temple user (mt.kb.user@gmail.com)
Subject: article: How to Trace a Email
Date: January 25, 2011 3:30:58 PM PDT
To: user@example.com
Return-Path: <mt.kb.user@gmail.com>
Envelope-To: user@example.com
Delivery-Date: Tue,

 25 Jan 2011 15:31:01 -0700
Received: from po-out-1718.google.com ([72.14.252.155]:54907) by cl35.gs01.gridserver.com with esmtp (Exim 4.63) (envelope-from <mt.kb.user@gmail.com>) id 1KDoNH-0000f0-RL for user@example.com; Tue, 25 Jan 2011 15:31:01 -0700
Received: by po-out-1718.google.com with SMTP id y22so795146pof.4 for <user@example.com>; Tue, 25 Jan 2011 15:30:58 -0700 (PDT)
Received: by 10.141.116.17 with SMTP id t17mr3929916rvm.251.1214951458741; Tue, 25 Jan 2011 15:30:58 -0700 (PDT)
Received: by 10.140.188.3 with HTTP; Tue, 25 Jan 2011 15:30:58 -0700 (PDT)
Dkim-Signature: v=1; a=rsa-sha256; c=relaxed/relaxed; d=gmail.com; s=gamma; h=domainkey-signature:received:received:message-id:date:from:to :subject:mime-version:content-type; bh=+JqkmVt+sHDFIGX5jKp3oP18LQf10VQjAmZAKl1lspY=; b=F87jySDZnMayyitVxLdHcQNL073DytKRyrRh84GNsI24IRNakn0oOfrC2luliNvdea LGTk3adIrzt+N96GyMseWz8T9xE6O/sAI16db48q4Iqkd7uOiDvFsvS3CUQlNhybNw8m CH/o8eELTN0zbSbn5Trp0dkRYXhMX8FTAwrH0=
Domainkey-Signature: a=rsa-sha1; c=nofws; d=gmail.com; s=gamma; h=message-id:date:from:to:subject:mime-version:content-type; b=wkbBj0M8NCUlboI6idKooejg0sL2ms7fDPe1tHUkR9Ht0qr5lAJX4q9PMVJeyjWalH 36n4qGLtC2euBJY070bVra8IBB9FeDEW9C35BC1vuPT5XyucCm0hulbE86+uiUTXCkaB 6ykquzQGCer7xPAcMJqVfXDkHo3H61HM9oCQM=
Message-Id: <c8f49cec0807011530k11196ad4p7cb4b9420f2ae752@mail.gmail.com>
Mime-Version: 1.0
Content-Type: multipart/alternative; boundary="----=_Part_3927_12044027.1214951458678"
X-Spam-Status: score=3.7 tests=DNS_FROM_RFC_POST, HTML_00_10, HTML_MESSAGE, HTML_SHORT_LENGTH version=3.1.7
X-Spam-Level: ***
Message Body: This is a KnowledgeBase article that provides information on how to find email headers and use the data to trace a email.
```

**How to analyze an email header**
CAUTION:

It is important to know that when reading an email header every line can be forged, so only the Received: lines that are created by your service or computer should be completely trusted.

- From: This displays who the message is from, however, this can be easily forged and can be the least reliable.
- Subject: This is what the sender placed as a topic of the email content.
- Date: This shows the date and time the email message was composed.
- To: This shows to whom the message was addressed, but may not contain the recipient's address.
- Return-Path: The email address for return mail. This is the same as "Reply-To:".
- Envelope-To: This header shows that this email was delivered to the mailbox of a subscriber whose email address is user@example.com.
- Delivery Date: This shows the date and time at which the email was received by your (mt) service or email client.
- Received: The received is the most important part of the email header and is usually the most reliable. They form a list of all the servers/computers through which the message traveled in order to reach you.

The received lines are best read from bottom to top. That is, the first "Received:" line is your own system or mail server. The last "Received:" line is where the mail originated. Each mail system has their own style of "Received:" line. A "Received:" line typically identifies the machine that received the mail and the machine from which the mail was received.

- Dkim-Signature & Domainkey-Signature: These are related to domain keys which are currently not supported by (mt) Media Temple services. You can learn more about these by visiting: http://en.wikipedia.org/wiki/DomainKeys.
- Message-id: A unique string assigned by the mail system when the message is first created. These can easily be forged.
- Mime-Version: Multipurpose Internet Mail Extensions (MIME) is an Internet standard that extends the format of email. Please see http://en.wikipedia.org/wiki/MIME for more details.
- Content-Type: Generally, this will tell you the format of the message, such as html or plaintext.
- X-Spam-Status: Displays a spam score created by your service or mail client.
- X-Spam-Level: Displays a spam score usually created by your service or mail client.
- Message Body: This is the actual content of the email itself, written by the sender.

**Finding the Original Sender**
The easiest way for finding the original sender is by looking for the X-Originating-IP header. This header is important since it tells you the IP address of the computer that had sent the email. If you cannot find the X-Originating-IP header, then you will have to sift through the Received headers to find the sender's IP address. In the example above, the originating IP Address is 10.140.188.3.

Once the email sender's IP address is found, you can search for it at http://www.arin.net/. You should now be given results letting you know to which ISP (Internet Service Provider) or webhost the IP address belongs. Now, if you are tracking a spam email, you can send a complaint to the owner of the originating IP address. Be sure to include all the headers of the email when filing a complaint.

The email body is the part of the email which contains the text (plain or HTML formatted) the sender wants you to view.

Below is an example of a text-only email.

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/94f3dd9b-728e-436f-9f39-715391f33242)

Below is an example of the HTML formatted email.

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/a40c4243-0029-462c-a37a-f003e65dea13)

The above email contains an

 image (which was blocked by the email client) and embedded hyperlinks.

HTML is what makes it possible to add these elements to an email.

To view an email's HTML code is the same approach shown below, but it may vary depending on the webmail client.

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/88c9ed6b-43b7-4b55-9cb6-abb10af0dc3a)

A snippet of the HTML code is shown below.

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/61e1f9bc-afd8-4e50-a6e6-80d154e4675e)

In this specific email web client, Protonmail, the option to switch back to HTML is called "View rendered HTML".

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/6ef22970-9800-47a8-a41e-d4c4c8e66bd2)

Again, it will be different for other webmail clients.

Lastly, emails may contain attachments. The same premise applies; you can view an email's attachment from an email's HTML format or by viewing the source code.

Let's look at a few examples below.

The following example is an HTML formatted email from "Netflix" with an attachment. The web client is Yahoo!

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/40580076-a451-48db-824d-a21572bfcd17)

The email body has an image.
The email attachment is a PDF document.
Now let's view this attachment within the source code.

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/91255ab6-5db2-4f1e-ab20-fd9b2718b40e)

From the above example, we can see the headers associated with this attachment:

- Content-Type is application/pdf.
- Content-Disposition specifies it's an attachment.
- Content-Transfer-Encoding tells us it's base64 encoded.

With the base64 encoded data, you can decode it and save it to your machine.

writing moree......
