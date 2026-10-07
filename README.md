# Mahuya Presales Manager

Tools and reports for the Alcove Realty presales team (New Kolkata: Sangam & Triveni).

## TAT Analysis Live

The TAT (turnaround time) dashboard reads the same leads three ways:

- **Employee site:** https://employees.alcoverealty.in/p/mahuya-2/ — press **Sign in with Salesforce** and log in with your usual Salesforce account. The page then queries Salesforce directly from your browser and refreshes every minute until 6:30 PM. You see only the leads your Salesforce access allows. Nothing is stored on the site or in this repo. *Needs the one-time admin setup below.*
- **claude.ai:** https://claude.ai/artifact/SyuL3hKto2duBxK2MdvXid uses each viewer's claude.ai Salesforce connector instead of a Salesforce sign-in.
- **Upload:** on either page, **Upload export** builds the report from the Salesforce lead export (`report*.xls`). The file is read in your browser and never uploaded anywhere. The export must include *Created Date(Considered for Report)*, *First Call Date Time*, *Lead Owner* and *Enquiry Type*.

## One-time Salesforce admin setup (for the employee site)

A Salesforce admin does this once, in **Setup** at https://alcoverealty.my.salesforce.com.

**1. Create the connected app.** Setup → App Manager → **New Connected App** (or **New External Client App**, if that's what your org offers).

| Setting | Value |
|---|---|
| Name | `TAT Analysis Live` |
| Enable OAuth Settings | On |
| Callback URL | `https://employees.alcoverealty.in/p/mahuya-2/` |
| Selected OAuth scopes | *Manage user data via APIs (api)* |
| Require Proof Key for Code Exchange (PKCE) | On |
| Require Secret for Web Server Flow | **Off**, because the page runs in the browser and can't keep a secret |
| Require Secret for Refresh Token Flow | Off |

Save, then open **Manage Consumer Details** and copy the **Consumer Key**. Under **Manage → Edit Policies**, set *Permitted Users* to "All users may self-authorize", or to "Admin approved users are pre-authorized" and add the Pre-Sales profiles.

**2. Allow the site in CORS.** Setup → **CORS**:
- Edit the settings and tick **Enable CORS for OAuth endpoints**.
- Add a new allowed origin: `https://employees.alcoverealty.in`

**3. Send the Consumer Key to Mahuya.** It goes into `SF_CLIENT_ID` near the top of the script in `tat-live.html`. The key is not a secret and is safe to commit. After that, rebuild `index.html`, push, and redeploy the site.

Until the key is added, the sign-in button says sign-in isn't set up yet, and upload still works.

## Files

| File | What it is |
|---|---|
| `tat-live.html` | Source. This is what gets published to claude.ai, which adds the page skeleton itself. |
| `index.html` | Standalone build of the same page, with a full `<html>`/`<head>`, served by the employee site. It's generated from `tat-live.html`, so edit the source and rebuild this one rather than editing it directly. |

## What it measures

Presales should make the first call on a fresh enquiry within **5 minutes** during office hours (10:00 AM – 6:30 PM IST).

- **Reporting window** for date D: leads with *Created Date (Considered for Report)* from D−1 6:30 PM to D 6:30 PM.
- **Scope:** Enquiry Type Inbound or Outbound only. Every lead owner with a lead in the window is included.
- **TAT** = First Call Date Time − Created Date (Considered for Report), in minutes.
- **Office Hours table:** 5-minute bands from 0–5 up to 70, then a single catch-all column. The table extends to 90 minutes if there are calls in those bands.
- **Outside Office Hours table** (6:30 PM – 10:00 AM): bands by the clock hour of the first call, 9–10 through 18–18:30.
- **Not called:** leads that have no First Call Date Time yet.

## Salesforce fields used (Lead object)

| Report column | API name |
|---|---|
| Created Date (Considered for Report) | `Enquiry_Date__c` |
| First Call Date Time | `First_Call_Date_Time__c` |
| Enquiry Type | `Enquiry_Type__c` |
| Lead Owner | `Owner.Name` |

## Updating the page

Edit `tat-live.html`, rebuild `index.html` from it, push, and redeploy the employee site. Republish the source to the same claude.ai artifact URL from Claude Code.

This repo is public, so never commit exported lead data (report*.xls, Excel workbooks, phone numbers or names of customers).
