# YourBabalawo.com Cloudflare D1 Database Version

This package upgrades YourBabalawo.com from a static landing page into a Cloudflare Pages site with a private D1 database for client intake and interaction tracking.

## Files included

- `index.html` — landing page with buttons routed to `intake.html`
- `intake.html` — client intake form that submits to `/api/intake`
- `payment.html`
- `prepare.html`
- `thank-you.html`
- `admin.html` — private admin screen for reviewing/updating intakes
- `schema.sql` — D1 database schema
- `functions/api/intake.js` — saves intake submissions into D1
- `functions/api/admin/intakes.js` — protected admin API for listing/updating intakes
- `functions/api/admin/events.js` — protected admin API for interaction logs
- `wrangler.toml.example`
- `_headers`

Keep `SwamiG.gif` in the repo root for the logo.

## Cloudflare setup

### 1. Upload files

Upload everything in this ZIP into the root of the YourBabalawo.com GitHub repository.

The folder structure must remain:

```text
functions/api/intake.js
functions/api/admin/intakes.js
functions/api/admin/events.js
schema.sql
admin.html
intake.html
index.html
payment.html
prepare.html
thank-you.html
```

### 2. Create the D1 database

In Cloudflare dashboard:

Workers & Pages → D1 → Create database

Suggested database name:

```text
yourbabalawo-db
```

### 3. Apply the schema

Open the D1 database console and paste/run the contents of:

```text
schema.sql
```

Alternative Wrangler command:

```bash
npx wrangler d1 execute yourbabalawo-db --remote --file=./schema.sql
```

### 4. Bind D1 to the Pages project

Go to:

Workers & Pages → your Pages project → Settings → Bindings → Add binding → D1 database

Use exactly this variable/binding name:

```text
DB
```

Select the D1 database you created.

### 5. Add an admin token

Go to:

Workers & Pages → your Pages project → Settings → Environment variables

Add this variable:

```text
ADMIN_TOKEN
```

Set it to a private password/token known only to you.

### 6. Redeploy

Redeploy the Cloudflare Pages project.

### 7. Test

Open:

```text
https://yourbabalawo.com/intake.html
```

Submit a test intake.

Then open:

```text
https://yourbabalawo.com/admin.html
```

Enter your `ADMIN_TOKEN` and load submissions.

## Important privacy note

Client spiritual intake information is personal and sensitive. Limit access to `admin.html`, use a strong ADMIN_TOKEN, and do not share it.
