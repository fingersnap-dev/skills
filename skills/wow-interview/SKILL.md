---
name: wow-interview
description: "Analyze a codebase and Git history to generate a comprehensive interview-prep document with likely interviewer questions, strong model answers, and follow-up talking points grounded in the repository."
license: Complete terms in LICENSE.txt
---

# Wow Interview

## Overview
Deep-dive into the entire codebase and Git history to prepare interviewer-style questions and strong model answers rooted in the actual repository. The goal is not generic framework trivia. The goal is to help the user explain what the system does, why it was designed this way, what trade-offs exist, how it evolved, and what they would improve next.

## Core Principles
- **Ground everything in evidence**: Questions and answers should point to real files, modules, tests, configs, or commits.
- **Prefer "why" over "what"**: Interviewers care more about trade-offs, constraints, and failure modes than basic syntax recall.
- **Breadth without fluff**: Generate as many meaningful questions as the repo supports, but collapse duplicates and empty variations.
- **Strong answers are honest**: Separate confirmed facts from inference. If an exact metric is unknown, say so and explain what is known.
- **Answer like a candidate, not a documentation bot**: Default to first-person answers such as "I chose..." or "I worked on..." unless authorship is clearly shared or unclear.
- **Own the rough edges**: Missing tests, fragile code, tech debt, or unfinished work are interview material too. Prepare direct, defensible answers.

## Workflow

### Step 0: Determine Preferred Language
Identify the user's preferred language for the output document:
- Explicit prompt instruction
- Guide files like `AGENTS.md`, `CLAUDE.md`, `.cursorrules`
- Language of the user's question

Write the final interview prep document in the user's preferred language.

### Step 1: Detect Project Stack and Interview Surface
Automatically identify the project's stack and the kind of interview questions it invites.

**Files to analyze:**
- `package.json`, `build.gradle`, `pom.xml`, `requirements.txt`, `Cargo.toml`, `Dockerfile`, Terraform files, CI configs, etc.

**Project type examples:**
- Backend service
- Frontend application
- Mobile app
- Library / SDK
- Infra / platform / DevOps
- Data / ML
- Internal tooling / automation
- Content-driven or meta repository (templates, plugins, skills, docs, schemas)

Also infer the likely candidate posture:
- Sole builder / owner
- Major contributor
- Feature owner inside a larger team
- Maintainer of an internal tool or library

This posture affects answer phrasing and confidence.

### Step 2: Exhaustive Codebase Analysis
Read the entire codebase line by line. Do not skim. This step focuses on the current system.

**Read everything relevant:**
- Source files, configuration, scripts, tests, and docs that affect implementation understanding
- Comments, TODOs, migration files, schema files
- CI/CD or release-related configs when present

**While reading, identify interview-worthy areas:**
- Architecture and module boundaries
- Domain model and data flow
- Non-trivial logic and algorithms
- External integrations and failure handling
- Performance-sensitive paths
- Testing strategy and blind spots
- Security, privacy, and operational concerns
- Design decisions that clearly imply trade-offs
- Places where an interviewer would naturally ask "why did you do it this way?"

**Output of this step:**
A repository-grounded list of technically interesting areas worth being questioned about

### Step 3: Deep Git History Analysis
Read the commit history thoroughly. This step focuses on system evolution and decision-making over time.

**3-1. Collect commit logs**
```bash
git log --oneline --all --pretty=format:"%h %s"
git log --stat
```

**3-2. Filter meaningful commits**
High-priority commits:
- `fix:` bug resolution
- `perf:` performance or scale improvements
- `refactor:` structural changes with reasoning value
- Large changes across many files
- Commits that add tests, observability, migrations, security controls, or release/process changes

Deprioritize:
- Typos
- Pure formatting
- Initial boilerplate unless it reveals intentional system shape

**3-3. Analyze diffs**
```bash
git show <hash>
git diff <before>..<after>
```

Extract:
- Before: what problem or limitation existed
- After: what changed
- Why: intent from commit message and surrounding context
- Interview story: what this says about debugging, prioritization, design judgment, or ownership

### Step 4: Build the Question Inventory
Generate as many meaningful interviewer questions as the repo supports. Do not cap the number artificially.

**Primary categories:**
- Project summary and product context
- Architecture and module boundaries
- Data flow / request lifecycle / state flow
- Key abstractions, interfaces, and separation of concerns
- Data modeling / storage / caching
- External integrations and operational dependencies
- Performance and scalability
- Reliability, failures, retries, and observability
- Testing and quality strategy
- Security and privacy
- Refactoring, maintainability, and technical debt
- Bug fixes, incidents, and debugging stories
- Team/process/review decisions visible from repo history
- Future improvements and roadmap
- Domain-specific deep dives unique to this project

**Domain-specific angle examples:**
| Project Type | Interview Angles |
|-------------|------------------|
| Backend | Consistency, transactions, latency, idempotency, scaling bottlenecks |
| Frontend | Rendering cost, state boundaries, loading strategy, accessibility, bundle size |
| Mobile | Offline behavior, battery, memory, background work, device constraints |
| Library / SDK | API ergonomics, versioning, backwards compatibility, extension points |
| Infra / DevOps | Deployment safety, rollback, observability, failure containment, cost |
| Data / ML | Data quality, reproducibility, pipeline reliability, model trade-offs |
| Meta / Content Repo | Packaging, discoverability, metadata design, authoring workflow, compatibility |

**Question quality rules:**
- Ask routine questions first, then deeper follow-ups
- Include "why X instead of Y?" whenever the repo contains enough evidence
- Include "what breaks first at scale?" when relevant
- Include "what would you improve next?" even when the code is already solid
- Include adversarial but fair questions about weaknesses, omissions, and trade-offs
- Prefer one strong question with follow-ups over many shallow paraphrases

**If the repo is content-heavy or meta rather than runtime-heavy:**
Shift the inventory toward:
- Packaging and distribution model
- Discovery and installation flow
- Authoring conventions
- Metadata design
- Compatibility constraints
- Maintenance and release workflow
- Why the repository is structured as content rather than executable code

### Step 5: Write Strong Model Answers
For every question, write a strong answer grounded in the repo.

**Answer structure:**
1. **Direct answer**: Lead with a clear thesis in 2-4 sentences
2. **Concrete evidence**: Mention files, modules, tests, configs, or commits
3. **Trade-off discussion**: Explain why this approach was chosen and what it costs
4. **Forward-looking angle**: State what you would improve, monitor, or revisit next

**Answer style rules:**
- Default to first-person singular: "I chose...", "I separated...", "I would improve..."
- If ownership is unclear or clearly shared, soften to "I worked on..." or "The repo suggests..."
- Never claim outcomes or authorship that the repo cannot support
- Mark uncertain statements as inference
- Do not invent metrics; if estimating, label them clearly
- Avoid generic framework praise unless it explains a concrete design choice

**High-value answer patterns:**
- "I optimized for X because the repo's current scale or constraints suggest Y"
- "I accepted this trade-off because it reduced complexity in A while making B an explicit follow-up"
- "The code shows this boundary because..."
- "If this had to scale further, the first pressure point would be..."

### Step 6: Prioritize, Deduplicate, and Stress-Test
After generating the full inventory:

1. Rank questions by `likelihood`, `difficulty`, and `value`
2. Deduplicate near-identical questions
3. Make sure each major subsystem has at least one question
4. Make sure the hardest questions have evidence-backed answers
5. Add follow-up questions where an interviewer would naturally push deeper
6. Identify gaps the user should verify manually before a real interview

**Good follow-up triggers:**
- Large or unusual modules
- Missing tests
- Hand-rolled abstractions
- Performance-sensitive code
- Integration boundaries
- Recent refactors or bug-fix clusters

### Step 7: Generate Final Output
Save the analysis results as **`interview-prep.md` in the project root**.

This file is preparation material, not a script to memorize word-for-word. It should give the user a large, repository-grounded bank of likely questions plus strong answer directions.

**Important:**
- Do NOT limit the number of useful questions
- Cover both easy and difficult questions
- Include both positive and uncomfortable questions
- Highlight unanswered risks the user should verify before interviewing

**File path:** `./interview-prep.md`

**Output format:**

````markdown
# [Project Name] Interview Preparation Document

## Project Snapshot

### Summary
[2-3 paragraphs describing what the project does, who it serves, and what role the candidate likely played]

### Tech Stack
| Category | Technologies |
|----------|-------------|
| Language | |
| Framework / Runtime | |
| Storage / Data | |
| Infrastructure / Tooling | |
| Testing / Quality | |

### Interview Posture
- Likely candidate role:
- Strongest ownership signals:
- Biggest technical stories:
- Biggest risk areas to prepare honestly:

---

## Must-Know Questions

Questions the candidate should be able to answer quickly and confidently.

### 1. [Question]

#### Why An Interviewer Would Ask This
[What the question is probing]

#### Model Answer
> [A concise, high-quality answer in the user's preferred language, written as the candidate would speak]

#### Repo Evidence
- `path/to/file`
- `path/to/other/file`
- `[hash]` - [commit summary]

#### Follow-up Questions
- [follow-up 1]
- [follow-up 2]

#### Pitfalls To Avoid
- [hand-wavy or misleading answer to avoid]

---

## Full Question Bank

### Category: Architecture

#### Q1. [Question]

**Priority**  
Likelihood: High | Difficulty: Medium | Value: High

**Why This Matters**  
[What capability or judgment this probes]

**Model Answer**  
> [A strong, concrete answer]

**Repo Evidence**
- `path/to/file`
- `path/to/file`
- `[hash]` - [commit summary]

**Follow-up Questions**
- [follow-up]
- [follow-up]

**Confidence**
- Confirmed / Inference / Needs verification

---

#### Q2. [Question]
[Same structure]

### Category: Data Flow
[Same structure]

### Category: Reliability
[Same structure]

### Category: Testing
[Same structure]

### Category: Trade-offs and Debt
[Same structure]

### Category: Future Improvements
[Same structure]

---

## Honesty Gaps To Verify Before Interview
- [fact or metric that is not fully provable from repo]
- [ownership ambiguity]
- [behavior that should be tested manually]

## Fast Drill Set
A short set of rapid-fire questions for rehearsal.

1. [Question]
   Answer outline: [1-2 sentence cue]
2. [Question]
   Answer outline: [1-2 sentence cue]
3. [Question]
   Answer outline: [1-2 sentence cue]
````

**Post-save message (in the user's preferred language):**
Inform the user that:
1. The interview prep document has been saved to `interview-prep.md`
2. The file contains both must-know questions and a larger categorized question bank
3. Every answer is grounded in repository evidence where possible
4. The `Honesty Gaps To Verify Before Interview` section marks facts they should double-check before a real interview

## Heuristics
- Large or unusual modules should produce both "how it works" and "why it is shaped this way" questions.
- Repositories with meaningful commit history should produce at least a few story-based questions: hard bugs, refactors, trade-offs, or design reversals.
- Missing tests, security controls, observability, or release automation should not be hidden; turn them into candid preparation questions.
- If the repo is a library or framework extension, prioritize API design, backwards compatibility, and ergonomics.
- If the repo is an internal tool, prioritize maintainability, adoption, trust, and failure recovery.
- If the repo is a content or metadata repository, prioritize packaging, discoverability, authoring conventions, and compatibility boundaries.
- A smaller number of well-grounded deep questions beats many generic trivia questions, but still aim for maximal meaningful coverage.
