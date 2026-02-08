# ClawGuru – SaaS + Conversation Mutant (Vercel + Netlify)

**Next.js 14.2.21** pinned (Netlify CVE gate safe), React 18.2.

ClawGuru ist kein „Blog“. Es ist ein Loop:
**Score → Runbook → Fix → Re-Check → Share**.

## Local run
```bash
cp .env.example .env.local
npm install
npm run dev
```

## Deploy

### Vercel
Repo importieren → Environment Variables setzen → Deploy.

### Netlify
- Build: `npm run build`
- Publish: `.next`
- Plugin: `@netlify/plugin-nextjs` (in `netlify.toml`)

## Stripe (nur Stripe)

**Pflicht:**
- `STRIPE_SECRET_KEY`
- `ACCESS_TOKEN_SECRET` (signiert den Access-Cookie)

**Prices (bereits vorkonfiguriert):**
- Pro: `price_1SyY02INFtiy8u5xA9v6IeVA`
- Team: `price_1SyY1FINFtiy8u5xGhxFTkEe`
- Day Pass: `price_1SyY2dINFtiy8u5xwInPbISw`

Wenn du willst, kannst du sie via ENV überschreiben:
- `STRIPE_PRICE_PRO`, `STRIPE_PRICE_TEAM`, `STRIPE_PRICE_DAYPASS`

## Wie der Zugang funktioniert (ohne Login)
1) User kauft via Stripe Checkout (`/api/stripe/checkout`)
2) Success Seite zeigt **„Zugriff aktivieren“**
3) `/api/auth/activate` verifiziert die Stripe Session und setzt einen **httpOnly Cookie** (`claw_access`)
4) `/dashboard` ist gated (Subscription-Status wird bei jedem Zugriff via Stripe geprüft)

## Produkte / Dateien
- Free Lead Magnet: `/public/downloads/clawguru-launch-pack.pdf`
- Pro/Team/Daypass Downloads (Placeholder, bitte ersetzen):
  - `/private_downloads/sprint-pack.pdf`
  - `/private_downloads/incident-kit.zip`

## Routen
- `/check` Live Security Check (Badge + Share Loop)
- `/copilot` Runbook Copilot (conversation engine)
- `/dashboard` Pro Area (gated)
- `/pricing` Pläne
- `/api/download?key=sprint-pack|incident-kit` gated download
- `/api/stripe/portal` Billing Portal

## Optional: LLM Copilot
Wenn `OPENAI_API_KEY` gesetzt ist, antwortet Copilot mit LLM.
Ohne Key bleibt er rule-based (kein Crash).
