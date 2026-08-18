# DevSynt AutoGram Engine (n8n Automation)

An automated social media publishing pipeline built using **n8n**, **Google Drive**, **Supabase**, **Google Gemini AI**, and the **LinkedIn REST API**.

The workflow polls a target Google Drive folder on a schedule, performs database-level duplicate checks via Supabase, uploads binary media to Supabase Storage, generates context-aware creative captions and hashtags using Google Gemini, and publishes live posts to LinkedIn with automated lifecycle status tracking.

---

## 🚀 Key Features

* **Automated Polling:** Cron-scheduled trigger checking Google Drive for new media uploads.
* **Deduplication Check:** Prevents duplicate processing by checking `drive_file_id` in Supabase before proceeding.
* **Cloud Storage Hosting:** Automatically uploads binary image assets to a public Supabase Storage bucket (`social-images`).
* **AI Copywriting & Tagging:** Google Gemini generates engaging captions, hooks, and relevant hashtags tailored to the asset name.
* **Automated Publishing:** Publishes updates to LinkedIn via the UGC Post REST API.
* **Lifecycle Logging:** Tracks post states in Supabase (`ready` → `published`) with UTC timestamps.

---

## 📁 Repository Structure

* `DevSynt AutoGram Engine.json`: Exported full n8n automation pipeline.
* `workflow_pipeline.png`: Visual diagram of all connected workflow nodes.
* `live_post_demo.png`: Screenshot of the live published LinkedIn post.
* `README.md`: Complete documentation and deployment guide.

---

## ⚙️ Workflow Architecture

```text
[Schedule Trigger]
        │
        ▼
[Google Drive: Fetch Images]
        │
        ▼
[Supabase: Deduplication Query (Get Many Rows)]
        │
        ▼
[If Node: Is Unprocessed?] ──False──► [Terminate Execution]
        │ True
        ▼
[Google Drive: Download Binary File]
        │
        ▼
[Supabase Storage: Upload Asset]
        │
        ▼
[Google Gemini API: Generate AI Caption & Tags]
        │
        ▼
[Supabase DB: Create Post Row (Status: ready)]
        │
        ▼
[LinkedIn REST API: Publish Live Post]
        │
        ▼
[Supabase DB: Update Record Status (Status: published)]
