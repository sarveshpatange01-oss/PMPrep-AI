# PM Interview Coach

AI-powered web application delivering product-management interview questions with instant, structured feedback.

**Live demo:** _add your Vercel URL here_
**Stack:** Next.js · React · Vercel · LLM API

---

## What it does

PM Interview Coach generates realistic product-management interview questions on demand — Product Sense, Strategy & Vision, Execution & Metrics, Behavioral & Leadership, and Estimation — and, once you answer, returns a structured evaluation instead of a vague "good job":

- **Four-dimension rubric** — Structure, Business Acumen, Prioritization, and Communication, each scored 1–5
- **An overall score out of 10**
- **Concrete strengths and improvement areas**, not generic praise
- **A one-line verdict** summarizing the answer the way a panelist would note it on a scorecard

The goal is to make practice feel closer to a real panel interview: same categories, same rubric-style scoring, immediate turnaround.

## How it works

1. You pick a **question category** and a **track** (APM / PM / Senior PM)
2. The app calls an LLM with a role-based system prompt to generate one non-generic question for that category and seniority level, returned as strict JSON
3. You write your answer in your own words
4. The app sends the question + your answer to the LLM with a second, evaluation-focused system prompt that enforces a fixed JSON schema (scores, strengths, improvements, verdict)
5. The response is parsed and rendered as a scorecard — no free-form prose feedback that's hard to act on

Both calls are constrained to return **strict JSON only**, which is what makes the feedback structured and renderable as a UI component rather than a wall of text.

## Tech stack

| Layer | Choice |
|---|---|
| Framework | Next.js (App Router) |
| UI | React |
| Hosting | Vercel |
| AI | LLM API (prompt-engineered, JSON-constrained outputs) |
| State | Client-side session state; no server-side user accounts |

## Prompt engineering approach

- **Role-based system prompts** — the model is framed as an interview panelist for generation and as a senior evaluator for feedback, which noticeably changes tone and rigor versus a single generic prompt
- **Category- and track-conditioned generation** — the prompt is parameterized by category and seniority so questions scale in difficulty instead of reusing one static bank
- **Schema-locked JSON output** — every response is required to match an exact JSON shape with no markdown or preamble, so the frontend can render it directly without brittle text parsing
- **Anti-repetition guardrails** — recent questions can be passed back into the prompt so the model avoids generating near-duplicates in a session

## Responsible AI practices

- Feedback is clearly labeled as **AI-generated and directional**, not a definitive interview verdict
- No personally identifiable information is required or stored — practice answers stay local to the session
- A lightweight **feedback rating** (👍/👎) is captured after each evaluation as a signal for reviewing where the model's grading feels off, rather than assuming the rubric is always correct
- Prompts are scoped narrowly to interview coaching; the model is not asked to make hiring, compensation, or other consequential decisions about real people

## Getting started

```bash
git clone <this-repo>
cd pm-interview-coach
npm install
```

Add your LLM API key to `.env.local`:

```
LLM_API_KEY=your_key_here
```

Run locally:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

Deploy on [Vercel](https://vercel.com) — connect the repo and set `LLM_API_KEY` as an environment variable in the project settings.

## Project structure

```
├── app/
│   ├── page.tsx              # main UI: setup, question card, scorecard
│   └── api/
│       ├── generate/route.ts # question generation endpoint
│       └── evaluate/route.ts # structured feedback endpoint
├── components/
│   ├── QuestionCard.tsx
│   ├── Scorecard.tsx
│   └── ProgressPanel.tsx
├── lib/
│   └── prompts.ts            # system prompts + JSON schema definitions
└── public/
```

## Roadmap

- [ ] Persist session history per user (auth + database)
- [ ] Export a session as a shareable PDF report
- [ ] Add a timed-response mode
- [ ] Expand rubric with a "clarifying questions asked" dimension for Product Sense
- [ ] Voice input for spoken-interview practice

## License

MIT — feel free to fork and adapt for your own interview prep.

## Author

