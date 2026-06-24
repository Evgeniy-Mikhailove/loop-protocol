# Loop Protocol

**Self-correcting iteration loops for AI output. Write, score, fix, repeat - until quality converges.**

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Variants](https://img.shields.io/badge/Variants-2_Loops-green.svg)](#two-variants)

---

## Problem

AI gives you a first draft. It's 70% good. You say "improve it." Now it's 75%. You say "more." It changes things that were already good and breaks them. After 3 rounds you're at 72% and frustrated.

The problem: no rubric, no score, no targeted fixes.

## Solution

A structured iteration loop with measurable quality. Each cycle scores against a rubric, identifies specific weaknesses, and rewrites only those parts. Stops automatically when quality converges.

---

## Two Variants

### For Code: loop-build

```
Spec → Build → Adversarial blind review → Fix → Repeat
```

The reviewer never sees the builder's reasoning - only the output. This prevents confirmation bias ("oh I see what they were trying to do, that's fine").

### For Content: content-loop

```
Generate rubric → Produce content → Score 0-100 → Identify weaknesses → Rewrite → Repeat
```

The rubric is generated FIRST, before writing anything. This anchors quality to concrete criteria, not vibes.

---

## Stop Conditions

Same for both variants:

| Condition | Threshold | Why |
|---|---|---|
| Quality reached | Score >= 95% | Good enough for production |
| Plateau detected | Score delta < 3% between iterations | No more improvement possible |
| Hard cap | Max 5 iterations | Prevent infinite loops |

Whichever triggers first, the loop stops.

---

## Content Loop - Step by Step

### Step 1: Generate Rubric

Before writing anything, define what "good" looks like:

```
Task: Write a product announcement for our new API feature

Rubric (auto-generated from task):
1. Clear value proposition in first sentence (0-20)
2. Technical accuracy of API description (0-20)
3. Concrete usage example included (0-20)
4. Call-to-action with next steps (0-20)
5. Appropriate tone for developer audience (0-20)
Total: /100
```

### Step 2: Produce First Draft

Write the content. Don't try to be perfect - just get it down.

### Step 3: Score Against Rubric

```
Scoring iteration 1:
1. Value proposition: 15/20 - present but buried in second paragraph
2. Technical accuracy: 18/20 - correct, minor terminology issue
3. Usage example: 8/20 - too abstract, needs real code
4. Call-to-action: 12/20 - vague "check it out" instead of specific link
5. Tone: 17/20 - good but slightly too casual

Total: 70/100
Weaknesses: #3 (usage example), #4 (CTA)
```

### Step 4: Targeted Rewrite

Fix ONLY the identified weaknesses. Don't touch what scored well.

```
Rewriting:
- #3: Replace abstract description with actual curl example
- #4: Add specific URL and "pip install" command

(Items #1, #2, #5 are left unchanged)
```

### Step 5: Re-score

```
Scoring iteration 2:
1. Value proposition: 15/20 (unchanged)
2. Technical accuracy: 18/20 (unchanged)
3. Usage example: 19/20 (real curl example added)
4. Call-to-action: 18/20 (specific install command + docs link)
5. Tone: 17/20 (unchanged)

Total: 87/100 (+17 from iteration 1)
Weaknesses: #1 (value prop still buried)
```

### Step 6: One More Iteration

```
Scoring iteration 3:
1. Value proposition: 19/20 (moved to first sentence)
2-5: (unchanged)

Total: 91/100 (+4)
Delta from last iteration: +4 (above 3% threshold, continue)
```

### Step 7: Final Iteration

```
Scoring iteration 4:
Total: 93/100 (+2)
Delta: +2 (below 3% threshold - PLATEAU)
STOP: quality has converged
```

Final output: 93/100, 4 iterations, each one targeted.

---

## Code Loop (loop-build) - Step by Step

### Step 1: Parse Spec into Checkable Requirements

```
Spec: "Build a rate limiter middleware for Express.js"

Requirements extracted:
1. [ ] Limits requests per IP per time window
2. [ ] Configurable window size and max requests
3. [ ] Returns 429 with Retry-After header when exceeded
4. [ ] Doesn't block legitimate traffic bursts (token bucket or sliding window)
5. [ ] Works with reverse proxy (X-Forwarded-For)
6. [ ] Has unit tests
```

### Step 2: Build

Write the implementation. First pass.

### Step 3: Adversarial Blind Review

A separate reviewer (fresh context, no access to the builder's reasoning) checks:

```
Review:
1. [PASS] Rate limiting works per IP
2. [PASS] Window and max are configurable
3. [FAIL] Returns 429 but no Retry-After header
4. [PASS] Sliding window implementation
5. [FAIL] Uses req.ip which doesn't respect X-Forwarded-For
6. [PASS] Tests exist but no edge case for proxy

Issues:
- Missing Retry-After header in 429 response
- req.ip doesn't work behind reverse proxy
- No test for proxy scenario
```

### Step 4: Fix

Fix only the identified issues. Don't refactor what passed.

### Step 5: Re-review

```
Review iteration 2:
1-6: [ALL PASS]
No issues found.
STOP: all requirements met
```

---

## Two-Agent Variant (for client-facing content)

For maximum quality on content that will be seen by customers:

```
Agent A (Writer): produces the content
Agent B (Blind Critic): scores against rubric WITHOUT seeing Agent A's process

Writer never sees Critic's internal reasoning
Critic never sees Writer's internal reasoning
```

This eliminates both confirmation bias (critic agreeing because they see the effort) and defensive rewrites (writer changing things to please the critic rather than improve quality).

---

## When to Apply

### Automatically apply loop-build when:
- Building from a spec with >3 requirements
- The spec has checkable acceptance criteria
- Code will go to production

### Automatically apply content-loop when:
- Content task where quality is subjective
- Client-facing content (emails, announcements, docs)
- Marketing copy, blog posts, product descriptions

### Skip the loop when:
- Single-file fixes, config changes
- Tasks under 10 minutes
- User explicitly asks for a quick draft
- Translations (quality is measurable but the loop adds little value)

---

## Integration with Other Patterns

| Used with | How |
|---|---|
| Adversarial pattern | The blind review IS the loop's quality check |
| Fanout & Synthesize | Loop each parallel branch independently |
| Prompt Chaining | Loop the final stage (generation), not extraction |
| Tournament | Loop the winning approach to polish it |

---

## License

Apache 2.0 - see [LICENSE](LICENSE) for details.
