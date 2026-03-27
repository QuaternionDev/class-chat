# class-chat

A real-time chat web app for classroom use. Students log in with their class account, pick a subject channel, and message each other in real time. Built with vanilla HTML/CSS/JS — no build step, no framework, just a single HTML file backed by Appwrite.

> **Note:** This repo is strongly a demo. You will need an Appwrite project to run this for real. Drop your credentials into the four constants at the top of the script and you're good. A preview mode is available with sample messages if you just want to explore the UI.

## How it works

1. Students log in with their class username and password (`username@feleles.local` under the hood)
2. The sidebar shows **groups** (Global, Group 1, Group 2) — clicking one slides open its channels
3. Picking a channel loads the message history and subscribes to real-time updates
4. Messages support markdown formatting, file attachments, and emoji reactions
5. Admins can create and delete channels, and delete any message

## Features

* 🔐 Appwrite email/password auth, scoped to a class domain
* 💬 Real-time messaging via Appwrite Realtime subscriptions
* 📁 Groups with sliding sub-channel navigation — no Discord-style icon rail
* ✍️ Markdown support — bold, italic, strikethrough, inline code, code blocks, blockquotes, lists
* 📎 File attachments — images (with lightbox), videos, and documents up to 50 MB
* 😊 Emoji reactions — six quick-reactions per message, toggle on/off
* ⚙️ Admin panel — create/delete channels, delete any message
* 👁 Preview mode — explore the full UI with sample messages, no account needed
* 📱 Mobile-friendly with a bottom navigation bar

## Project structure

```
oral-draw/
└── index.html    # Full frontend — single self-contained file
```

---

## Trying it out

Open `index.html` in a browser and hit **Előnézet fiók nélkül →** on the login screen. You'll get the full UI with sample messages and admin access, no account or backend needed.

To host it as a static page, any static file host works — just upload `index.html`:

* **Netlify** — drag and drop the file at [netlify.com](https://netlify.com)
* **GitHub Pages** — push to a repo, enable Pages under Settings
* **Vercel** — import the repo at [vercel.com](https://vercel.com)
* **Replit** — create a new HTML/CSS/JS repl and replace the default `index.html`

---

## Connecting a backend

You need an Appwrite project with the following set up:

**Database collections** (attribute names must match exactly):

- `channels` — `name`, `group`, `description`, `created_by`
- `messages` — `channel_id`, `user_id`, `user_name`, `content`, `file_ids` *(nullable)*, `file_meta` *(nullable)*
- `reactions` — `message_id`, `channel_id`, `user_id`, `user_name`, `emoji`

**Storage** — one bucket for file attachments.

**Users** — create accounts via the Appwrite console. The app strips the domain on login, so a student types `kovacs.balazs` and the app authenticates as `kovacs.balazs@feleles.local`.

Then open `index.html` and fill in the four constants near the top of the `<script>` block:

```js
const ENDPOINT   = 'https://cloud.appwrite.io/v1';
const PROJECT_ID = 'YOUR_PROJECT_ID';
const DB_ID      = 'YOUR_DATABASE_ID';
const BUCKET_ID  = 'YOUR_BUCKET_ID';
const DOMAIN     = '@feleles.local'; // appended to username on login
```

On first login as an admin, the default channels are seeded automatically if the `channels` collection is empty.

---

## Admin setup

Grant the `admin` label to a user in the Appwrite console:

**Users → select user → Labels → add** `admin`

Admins can create and delete channels, and delete any message in any channel.

---

## Customisation

A few things you might want to tweak:

* **Groups** — edit the `GROUPS` array to rename groups or add new ones.
* **Default channels** — edit `DEFAULT_CHANS` to change what gets seeded on first run.
* **Class domain** — change the `DOMAIN` constant to match your school's email suffix.
* **Quick-reactions** — edit the `QUICK_EMOJIS` array to swap in different emojis.
* **School name** — change the eyebrow text on the login screen (`Széchenyi SZC · Osztálychat`).

---

## Planned

* Push notifications for new messages
* Unread message indicators per channel
* Message editing

---

## Disclaimer

This project was built with the assistance of Claude by Anthropic. The code, documentation, and design were generated and iterated through a conversational development process. Most of this repo is vibe-coded.

---

## License

MIT
