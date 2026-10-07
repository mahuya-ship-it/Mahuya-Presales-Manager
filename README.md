# Mahuya Presales Manager

Tools and reports for the Alcove Realty presales team (New Kolkata: Sangam & Triveni).

## TAT Analysis Live

The TAT (turnaround time) dashboard works in two ways:

- **Live, on claude.ai:** https://claude.ai/artifact/SyuL3hKto2duBxK2MdvXid reads leads straight from Salesforce through each viewer's own claude.ai Salesforce connector and refreshes every minute until 6:30 PM.
- **Standalone HTML:** [`tat-analysis/index.html`](tat-analysis/index.html) is a complete page that opens in any browser. Download it, open it, and press **Upload export** to load the Salesforce lead export (`report*.xls`). The file is read in your browser and never uploaded anywhere. The page sets the report date from the latest lead in the file, and you can pick another date.

The export must include the columns *Created Date(Considered for Report)*, *First Call Date Time*, *Lead Owner* and *Enquiry Type*. If any of them is missing, the page names the missing columns and asks for a re-export.

### Files

| File | What it is |
|---|---|
| `tat-analysis/tat-live.html` | Source. This is what gets published to claude.ai, which adds the page skeleton itself. |
| `tat-analysis/index.html` | Standalone build of the same page, with a full `<html>`/`<head>`. It's generated from `tat-live.html`, so edit the source and rebuild this one rather than editing it directly. |

### What it measures

Presales should make the first call on a fresh enquiry within **5 minutes** during office hours (10:00 AM – 6:30 PM IST).

- **Reporting window** for date D: leads with *Created Date (Considered for Report)* from D−1 6:30 PM to D 6:30 PM.
- **Scope:** Enquiry Type Inbound or Outbound only. Every lead owner with a lead in the window is included.
- **TAT** = First Call Date Time − Created Date (Considered for Report), in minutes.
- **Office Hours table:** 5-minute bands from 0–5 up to 70, then a single catch-all column. The table extends to 90 minutes if there are calls in those bands.
- **Outside Office Hours table** (6:30 PM – 10:00 AM): bands by the clock hour of the first call, 9–10 through 18–18:30.
- **Not called:** leads that have no First Call Date Time yet.

### Salesforce fields used (Lead object)

| Report column | API name |
|---|---|
| Created Date (Considered for Report) | `Enquiry_Date__c` |
| First Call Date Time | `First_Call_Date_Time__c` |
| Enquiry Type | `Enquiry_Type__c` |
| Lead Owner | `Owner.Name` |

### Updating the live page

Edit `tat-analysis/tat-live.html`, rebuild `index.html` from it, then republish the source to the same artifact URL from Claude Code.

This repo is public, so never commit exported lead data (report*.xls, Excel workbooks, phone numbers or names of customers).
