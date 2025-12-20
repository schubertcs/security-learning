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

## The Demo

Interactive example: this simulates how a vulnerable application might respond to different URLs. No real requests are made.

<div class="fake-browser">
  <div class="browser-bar">
    <input
      id="fake-url"
      type="text"
      value="/christoph-schubert/documents/bank-statements"
    />
    <button id="go-btn" aria-label="Navigate">→</button>
  </div>

  <div id="fake-content" class="browser-content"></div>
</div>

<style>
.fake-browser {
  border: 1px solid var(--border);
  border-radius: 6px;
  overflow: hidden;
  margin: 1.5rem 0;
  background: var(--entry);
  font-family: system-ui, sans-serif;
}

.browser-bar {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem;
  background: var(--theme);
  border-bottom: 1px solid var(--border);
}

.browser-bar button {
  padding: 0.25rem 0.5rem;
  font-size: 1rem;
  cursor: pointer;
  background: var(--entry);
  color: var(--primary);
  border: 1px solid var(--border);
  border-radius: 4px;
}

.browser-bar button:hover {
  background: var(--secondary);
  color: var(--theme);
}

.browser-bar input {
  flex: 1;
  padding: 0.4rem;
  font-family: monospace;
  background: var(--entry);
  color: var(--primary);
  border: 1px solid var(--border);
  border-radius: 4px;
  box-shadow: inset 0 0 0 1px var(--border);
}

.browser-bar input::placeholder {
  color: var(--secondary);
}

.browser-content {
  padding: 1rem;
  background: var(--entry);
  color: var(--primary);
  min-height: 4rem;
}
</style>

<script>
  const content = document.getElementById("fake-content");
  const input = document.getElementById("fake-url");
  const button = document.getElementById("go-btn");

  function navigate() {
    const url = input.value.trim();

    if (url === "/christoph-schubert/documents/bank-statements") {
      content.innerHTML =
        "<strong>Bank Statements</strong><br>January.pdf<br>February.pdf";
    } else if (url.startsWith("/jeff-bezos")) {
      content.innerHTML =
        "<strong>Jeff Bezos</strong><br>taxes.xlsx<br>private-notes.txt";
    } else {
      content.innerHTML = "<em>404 Not Found</em>";
    }
  }

  button.addEventListener("click", navigate);
  input.addEventListener("keydown", e => {
    if (e.key === "Enter") navigate();
  });

  navigate();
</script>


## The Impact
This primarily leads to unauthorized data access. Depending on the exposed functionality, attackers may also be able to perform actions on behalf of other users or escalate their privileges.

## The Takeaway
If something looks easy to enumerate, attackers will almost certainly try. Lists of usernames can be bought online, and directories can be programmatically enumerated. This means the barrier for attackers to carry out this attack is very low.

## Related Topics
- Path Traversal
- Access Restriction