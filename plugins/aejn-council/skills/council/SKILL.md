---
name: council
description: Run a council of independent AI advisors (Believer, Skeptic, Investor, Contrarian) in parallel on an idea, plan, price or decision, then have a separate Judge deliver one honest verdict. Use when the user types /council, or asks for an honest, brutal, second-opinion, "is this a good idea", "should I do X", or pros-and-cons verdict on a business idea, product, price, project, or strategy. Built to stop yes-man answers.
---

# Council

One model asked "is this a good idea?" tends to agree with the person asking. The council fixes that by giving the same question to advisors who are each paid to see it differently, keeping them isolated so they can't anchor on each other, and letting a Judge who wrote none of the arguments make the call.

A run uses 5 agents. If the user asked for the council by name, go ahead. Otherwise say it uses 5 agents and ask before starting. If the question is trivial, offer a direct answer instead.

## 1. Frame the question

Turn the user's words into a brief every advisor receives verbatim:

- **The decision:** one sentence, phrased as a choice ("Launch a paid plan now, or grow the free version first?").
- **Context:** only what the user has said about this decision. Do not go looking through memory, other chats or files. Leave out secrets and personal details the decision doesn't need. Never invent numbers; list unknowns as unknowns.
- **What a good outcome looks like** for the user, in their terms (money, time, reputation, fun).

If the question is too vague to judge (no idea what's being decided), ask one clarifying question first. Otherwise don't stall.

## 2. Convene the advisors (parallel, isolated)

Launch all four with the Agent tool **in a single message** so they run in parallel (`subagent_type: "general-purpose"`, `run_in_background: false`). Each gets the framed brief plus its persona below, and none sees another's output. Tell each: if web search is available, use it for claims that depend on current facts (market size, competitor prices, demand) without putting private details in searches, cite what they found, and label guesses as guesses.

- **The Believer:** Build the strongest honest case that this works. Where is the real upside, who exactly buys, and what has to go right? No cheerleading without evidence.
- **The Skeptic:** Argue from the customer's side why they won't pay, won't switch, or won't care. Find the objection that kills it, the competitor who already owns it, and the demand that isn't really there.
- **The Investor:** Only the numbers. Price, realistic volume, time cost in hours, running costs, payback, and what else that time could earn. State every assumption. If the math can't be done, say what data is missing.
- **The Contrarian:** Question the framing itself. Is this the wrong question? What option isn't on the table? What would a smart person do instead, and what second-order effect is everyone missing (platform rules, legal exposure, chargebacks, burnout, reputation)?

Every advisor returns exactly this, under 250 words:

```
VERDICT: GO / GO WITH CHANGES / NO — score 1-10
STRONGEST POINTS: 3 bullets, each with evidence or marked [guess]
BIGGEST RISK: one sentence
WHAT WOULD CHANGE MY MIND: one sentence
```

## 3. The Judge

After all four return, launch one more Agent (`general-purpose`, `run_in_background: false`) as **The Judge**. Give it the framed brief and the four reports, labelled by persona, and nothing else from this conversation. Its instructions: weigh evidence over conviction, discount unsupported claims, name where advisors agree (that's signal) and where they conflict, and make one call. No fence-sitting: "it depends" must name what it depends on and which way to go today.

The Judge returns:

```
FINAL VERDICT: GO / GO WITH CHANGES / NO   (confidence: low / medium / high)
THE CALL: 2-3 sentences, plain words
WHY: the 3 deciding factors
IF YOU GO: the first 3 concrete steps, smallest first
KILL SIGNAL: the one thing that should make the user stop
```

## 4. Report to the user

Lead with the Judge's verdict and the call. Then a compact table of the four advisors (persona · verdict · score · one-line reason), then the Judge's steps and kill signal. Keep sources the advisors cited as links at the end. Don't soften the verdict or add your own spin on top of the Judge. If you disagree with it on facts you can verify, say so separately and briefly, with the evidence.
