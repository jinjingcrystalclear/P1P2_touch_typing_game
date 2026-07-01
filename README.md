# PinPinPanda V6 P1/P2 School Edition

## Scope
- P1 uses only 1A word bank.
- P2 uses only 2A word bank.
- Class options: P1 CL1–CL5, P2 CL1–CL4.
- Student interface does not show backend curriculum rules.
- Level 1: single characters only.
- Level 2: phrases only.
- Level 3: short sentences only.
- Questions are shuffled every time and do not repeat within the round as far as possible.
- End screen shows tone-mark pinyin.
- Wrong answers are recorded in the report and can be submitted to Google Sheets.

## Upload to GitHub Pages
Upload `index.html` to the root of your GitHub repository.
Then go to Settings → Pages → Deploy from branch → main → root.

## Google Sheets Setup

Create a Google Sheet with headers:

Timestamp | Name | Class | Grade | Score | Accuracy | Correct | Wrong | Total | Wrong Words | Details

Go to Extensions → Apps Script and paste this:

```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data = JSON.parse(e.postData.contents);

  sheet.appendRow([
    new Date(),
    data.name || "",
    data.class || "",
    data.grade || "",
    data.score || 0,
    data.accuracy || 0,
    data.correctCount || 0,
    data.wrongCount || 0,
    data.totalCount || 0,
    data.wrongWords || "",
    JSON.stringify(data.details || [])
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({status: "ok"}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

Deploy as Web App:
- Execute as: Me
- Who has access: Anyone

Copy the Web App URL and paste it inside `index.html`:

```javascript
const GOOGLE_SCRIPT_URL = "PASTE_YOUR_URL_HERE";
```
