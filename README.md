# VoiceCheck: setup guide (100% free tiers)

A free web app that highlights AI-sounding sentences and rewrites them in a more natural voice.
Static frontend (`public/`) plus two serverless functions (`api/`) that call Google's Gemini API.
Your key stays on the server, never in the browser.

You need: a Google account, a GitHub account, and a computer (a phone works but uploading folders is fiddly; a library or friend's computer is fine for this one-time setup).

## Step 1: Get a free Gemini API key (2 minutes)
1. Go to aistudio.google.com/apikey and sign in with Google.
2. Click **Create API key** and copy it. Keep it private.

## Step 2: Create a free Upstash Redis database (3 minutes)
This makes the per-visitor daily limits reliable.
1. Sign up at upstash.com and click **Create database** (Redis, free plan, any region).
2. On the database page, find the **REST API** section and copy `UPSTASH_REDIS_REST_URL` and `UPSTASH_REDIS_REST_TOKEN`.

## Step 3: Put the project on GitHub (5 minutes)
1. Unzip `voicecheck-site.zip`.
2. On github.com click **New repository**, name it `voicecheck`, create it.
3. Click **uploading an existing file**, drag in everything from inside the unzipped folder (keep the `api` and `public` folders), then **Commit changes**.

## Step 4: Deploy on Vercel (5 minutes)
1. Sign up at vercel.com with your GitHub account (the free Hobby plan is fine).
2. Click **Add New > Project**, pick the `voicecheck` repo, and click **Import**.
3. Before deploying, open **Environment Variables** and add:
   - `GEMINI_API_KEY` = your key from step 1
   - `UPSTASH_REDIS_REST_URL` = from step 2
   - `UPSTASH_REDIS_REST_TOKEN` = from step 2
4. Click **Deploy**. In a minute you get a public link like `voicecheck.vercel.app`.

## Step 5: Test it
1. Open your link and click **Try a sample**, then **Check text**. The first sentences should highlight.
2. Click **Rewrite flagged sentences** and confirm you get revised text.
3. If anything fails, see Troubleshooting below.

## Step 6 (optional): Your own domain
Vercel > Project > Settings > Domains. A domain costs money; the free `.vercel.app` link works fine.

## How the free limits work
| Setting | Default | Meaning |
|---|---|---|
| `PER_IP_DAILY_WORDS` | 3000 | Words each visitor can process per day |
| `GLOBAL_DAILY_WORDS` | 60000 | Daily cap for the whole site, to stay inside Gemini's free quota |
| `GEMINI_MODEL` | gemini-3.1-flash-lite | If this model isn't available to you, try `gemini-2.5-flash-lite` |

Change them in Vercel > Settings > Environment Variables, then redeploy. Google's free quota is shared by all your visitors and can change without notice. When it runs out, visitors see a "busy, try again" message. Check your current limits at aistudio.google.com/rate-limit and adjust `GLOBAL_DAILY_WORDS` to match.

## Troubleshooting
- **"The server is not configured yet"**: `GEMINI_API_KEY` is missing. Add it in Vercel and redeploy.
- **"The analysis service had a problem"**: the model name may be wrong for your account. Set `GEMINI_MODEL` to `gemini-2.5-flash-lite` and redeploy.
- **"The free AI service is busy"**: you hit Google's free rate limit. Wait a minute, or lower the limits.
- **"Usage tracking is unavailable"**: the Upstash values are wrong or missing.

## Privacy and terms
- On Google's free tier, text sent to the API may be used to improve Google's products. The footer says so. Don't remove that notice.
- Read Google's current Gemini API terms before adding ads or charging users; free-tier terms can restrict commercial use.
- Scores are model estimates, not Turnitin results. Keep the footer disclaimer.
