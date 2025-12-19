---
title: "Forced Browsing"
date: 2025-12-19T21:21:02+01:00
tags:
  - attack
---

## The Setup
Assume we have a browser-based storage application similar to Google Drive or OneDrive in front of us. As the user _Christoph Schubert_ you might see this URL in your address bar when looking at the folder `documents/bank-statements`:

```url
https://www.my-webstorage.com/christoph-schubert/documents/bank-statements
```

## The Attack
Taking a deeper look at this URL you can see the username `christoph-schubert` in there. An attacker will now try to replace this valid username with other values, hoping that they might be able to see the data of that new, guessed user. If the backend does not properly verify that the authenticated user is allowed to access the requested resource, the request may succeed.

For example, the attacker might try:

```url
https://www.my-webstorage.com/jeff-bezos
https://www.my-webstorage.com/jeff-bezos/documents
https://www.my-webstorage.com/jeff-bezos/pictures
https://www.my-webstorage.com/ryan-reynolds
https://www.my-webstorage.com/ryan-reynolds/documents
https://www.my-webstorage.com/ryan-reynolds/pictures
...
```

At some point they might be lucky and find some existing username and can then freely access that user's data.

## The Impact
This primarily leads to unauthorized data access. Depending on the exposed functionality, attackers may also be able to perform actions on behalf of other users or escalate their privileges.

## The Takeaway
If something looks easy to enumerate, attackers will almost certainly try. Lists of usernames can be bought online, and directories can be programmatically enumerated. This means the barrier for attackers to carry out this attack is very low.

## Related Topics
- Path Traversal
- Access Restriction