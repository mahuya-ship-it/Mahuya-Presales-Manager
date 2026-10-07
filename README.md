# Mahuya Presales Manager

Tools and reports for the Alcove Realty presales team (New Kolkata: Sangam & Triveni).

## TAT Analysis Live

[`tat-analysis/tat-live.html`](tat-analysis/tat-live.html) is the source for the live TAT (turnaround time) dashboard.

**Open the live version here:** https://claude.ai/artifact/SyuL3hKto2duBxK2MdvXid

The page reads leads from Salesforce through each viewer's own claude.ai Salesforce connector. Opened anywhere else, such as from this repo or as a local file, it loads without data and says live data isn't available.

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

Edit `tat-analysis/tat-live.html`, then republish it to the same artifact URL from Claude Code.

This repo is public, so never commit exported lead data (report*.xls, Excel workbooks, phone numbers or names of customers).
