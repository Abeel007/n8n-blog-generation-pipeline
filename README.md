# n8n-blog-generation-pipeline
Automated blog generation and publishing pipeline built in n8n — from idea to live post in under 5 minutes.

## About the Project

Content marketing is one of the most expensive invisible costs for any business that runs on publishing. Writing a blog post the traditional way means researching the topic, drafting the body copy, writing a LinkedIn teaser, adding hashtags, writing a CTA, formatting everything for the website, uploading it to the CMS, and then separately logging into LinkedIn to post. That is easily an hour or more of skilled time per post, and most of it is repetitive mechanical work.

This system eliminates all of that. A team member submits a one-line idea through a form. Claude researches the topic in real time, writes the full blog post, generates a LinkedIn teaser, hashtags, and a call-to-action, stores everything in a Google Doc, and sends it to Slack for a one-click approval. One button push publishes it to the website and posts to LinkedIn simultaneously. The entire human effort is one form submission and one Slack button click.

---

## Core Features

- **AI-Powered Blog Writing with Live Research:** Claude Sonnet 4.5 researches the topic in real time via SearchAPI.io before writing, producing content grounded in current information rather than training data alone.
- **One-Click Slack Approval Flow:** Every generated blog lands in Slack with the full preview and three action buttons: Approve Auto, Approve Website Only, or Regenerate. No context switching, no extra tools.
- **Self-Healing JSON Validation:** A custom retry loop catches malformed AI responses automatically, re-requests from Claude, and logs failures to Google Sheets without any manual intervention.
- **Full Pipeline Tracking in Google Sheets:** Every blog is tracked from idea submission through to published URLs, with status, timestamps, Google Doc links, and LinkedIn post URLs all logged in real time.
- **Simultaneous Multi-Platform Publishing:** One approval publishes to the website via JWT-authenticated REST API and posts to LinkedIn via the UGC API at the same time.

---

## Built With

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Automation** | n8n | Workflow orchestration and logic |
| **AI Model** | Claude Sonnet 4.5 (Anthropic) | Blog writing and content generation |
| **Web Research** | SearchAPI.io | Live topic research before writing |
| **Approval Interface** | Slack Interactive Messages | Human-in-the-loop approval and publishing |
| **Document Storage** | Google Docs | Blog draft storage and review |
| **Pipeline Tracking** | Google Sheets | Status management and audit trail |
| **Website Publishing** | Custom REST API + JWT Auth | Automated blog post publishing |
| **Social Publishing** | LinkedIn UGC API | Automated LinkedIn post creation |
| **Logic Layer** | JavaScript (n8n Code Nodes) | Custom parsing, validation, and retry logic |

---

## Getting Started

### Prerequisites
- n8n instance (self-hosted or cloud)
- Anthropic API key (Claude Sonnet 4.5 access)
- SearchAPI.io API key
- Slack workspace with a bot token and interactive components enabled
- Google account with Drive and Sheets API access
- LinkedIn Developer App with `w_member_social` OAuth scope
- Website REST API credentials with JWT authentication

### Installation

1. **Import the workflow**
   - Open your n8n instance
   - Go to Workflows and click Import
   - Upload `Blog_Generation_System.json`

2. **Set up credentials in n8n**
   - Anthropic API: add your API key under Credentials
   - Google Sheets and Google Docs: connect via OAuth2
   - Slack: add your bot token and signing secret
   - LinkedIn: connect via OAuth2 with `w_member_social` scope
   - SearchAPI.io: add your API key as an HTTP Header credential

3. **Configure environment variables**
   Set the following directly in the relevant n8n nodes:

   | Variable | Where | Description |
   |----------|-------|-------------|
   | `SLACK_SIGNING_SECRET` | Slack Webhook node | For verifying incoming Slack requests |
   | `GOOGLE_SHEET_ID` | All Google Sheets nodes | ID of your tracking spreadsheet |
   | `GOOGLE_DOC_TEMPLATE_FOLDER` | Google Docs node | Drive folder ID for storing drafts |
   | `WEBSITE_API_URL` | HTTP Publish node | Your website REST API endpoint |
   | `WEBSITE_JWT_SECRET` | JWT Token node | Secret for signing publish requests |
   | `LINKEDIN_PERSON_URN` | LinkedIn node | Your LinkedIn person URN for posting |

4. **Set up the Google Sheet**
   Create a sheet named `BLog-post-2` with these columns:
   `blog_id`, `idea`, `title`, `description`, `post`, `hashtags`, `call_to_action`, `linkedin_teaser`, `status`, `drive_doc_url`, `published_at`, `linkedin_post_url`, `claude_retry`

5. **Configure the Slack webhook**
   - Go to your Slack App settings and enable Interactivity
   - Set the Request URL to your n8n webhook URL for the approval workflow
   - Set the approval Slack message trigger channel in the workflow

6. **Activate the workflow**
   - Turn on both the main generation workflow and the Slack approval workflow in n8n

---

## Usage Guide

### How to Generate and Publish a Blog Post

1. **Submit an idea:** Open the n8n form link and fill in the blog idea field. Hashtags, reference URL, and image URL are optional.
2. **Wait for the Slack message:** Within a few minutes a formatted message appears in your designated Slack channel showing the blog title, description, LinkedIn teaser, preview image, and a link to the full Google Doc.
3. **Review the draft:** Click the Google Doc link to read the full post.
4. **Choose an action:**
   - **Approve Auto:** Publishes to the website and posts to LinkedIn simultaneously
   - **Approve Website:** Publishes to the website only
   - **Regenerate:** Discards the draft and reruns the AI generation from scratch
5. **Confirm publishing:** A Slack thread reply confirms the live URLs the moment publishing completes.

### Tracking the Pipeline

Open the `BLog-post-2` Google Sheet at any time to see the full status of every blog post including drafts, pending approvals, published posts, and any failed generations.

---

## Roadmap

- [x] AI blog generation with live web research
- [x] Google Docs draft storage
- [x] Slack approval with three-path routing
- [x] Simultaneous website and LinkedIn publishing
- [x] Self-healing JSON retry loop
- [x] Full audit trail in Google Sheets

---

## Contact & Team

Built by **Codoro** - AI Automation Agency

| | |
|--|--|
| Website | [codoroai.com](https://codoroai.com) |
| Email | services@codoroai.com |
| LinkedIn | [Codoro on LinkedIn](https://www.linkedin.com/company/codoro) |
| Upwork | [Hire us on Upwork](https://www.upwork.com/freelancers/~01f0abdab02d0a6daa) |
