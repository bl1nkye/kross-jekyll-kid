# Google Script Instruction and Sitepins Basic Tutorial

## Goal
Provide a simple path for publishing blog posts by:
1) collecting content via a Google Form, and
2) automatically sending that content to the site via a webhook.

---

## Option A: Google Forms + Google Apps Script (recommended)
### 1) Create the Google Form
- Add fields like **Title**, **Author**, **Content**, **Tags**, and **Cover Image URL**.
- Make sure each field has a clear label (the script will reference these labels).

### 2) Link the Form to a Google Sheet
- In the Form, click **Responses** → **Link to Sheets**.
- This Sheet stores all submissions.

### 3) Add Google Apps Script
- In the Sheet, go to **Extensions → Apps Script**.
- Create a script file (e.g., `submitToWebhook.gs`).
- Use an on-submit trigger to run your script every time a new entry arrives.

**Example structure (pseudo-code):**
```javascript
function onFormSubmit(e) {
  const values = e.namedValues;
  const payload = {
    title: values["Title"][0],
    author: values["Author"][0],
    content: values["Content"][0],
    tags: values["Tags"][0]
  };

  UrlFetchApp.fetch("YOUR_WEBHOOK_URL", {
    method: "post",
    contentType: "application/json",
    payload: JSON.stringify(payload)
  });
}
```

### 4) Create the Trigger
- In Apps Script: **Triggers → Add Trigger**.
- Choose `onFormSubmit` as the function.
- Event type: **From spreadsheet → On form submit**.

### 5) Webhook Receiver (Site Update)
- Your webhook should convert the payload into a new Jekyll post file (e.g., `_posts/YYYY-MM-DD-title.md`).
- It should include front matter (title, author, tags) and the content body.

---

## Option B: Sitepins Basic Tutorial (alternative)
> Sitepins is a no-code tool for updating static sites by connecting forms to data pipelines.

### 1) Create a Sitepins Account
- Sign up and create a new project.

### 2) Connect a Form
- Use Sitepins' built-in form connector or link a Google Form.
- Map form fields to your content schema (title, content, tags, etc.).

### 3) Configure Output
- Set the output template for Jekyll posts, including front matter.
- Example front matter:
```yaml
---
title: "{{title}}"
author: "{{author}}"
date: "{{date}}"
tags: [{{tags}}]
---
```

### 4) Publish to GitHub
- Connect Sitepins to your GitHub repo.
- Configure it to create a new post file in `_posts/` with the date-based filename.

---

## Tips for Choosing a Stack
- **Choose Google Apps Script** if you want full control and already use Google Forms.
- **Choose Sitepins** if you want a no-code workflow and a visual interface.

## Next Steps Checklist
- [ ] Decide on Google Script vs. Sitepins.
- [ ] Confirm your required post fields.
- [ ] Add a webhook endpoint to publish new posts.
- [ ] Test with a sample submission.
