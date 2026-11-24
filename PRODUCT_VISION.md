# Evercanon — Product Vision

**Agents change. Context stays.**

*A repository-native framework for human–AI collaboration and continuous project context.*

**Status:** Initial product concept, 24 November 2026. This document captures the founding discussion so that development can continue in a fresh conversation. It is not an implementation specification, a claim of existing functionality, or a commitment to a particular architecture.

## 1. The idea

Evercanon makes the repository the durable source of truth for a project developed with AI.

A human and an AI product partner can discuss the product over many conversations, explore alternatives, challenge each other's assumptions and agree on what to build. Their conclusions become living documents in the repository as soon as they are sufficiently clear. Coding agents then work on individual tasks using that current repository context, rather than relying on a long chat transcript or a particular agent's memory.

The human can switch models, start new conversations, replace a coding agent, or deliberately give every pull request a fresh agent session without losing the project's identity, reasoning or accumulated knowledge.

The core idea is not to make an AI remember everything. It is to make the important things independent of any AI's memory.

Evercanon should eventually be reusable across projects, not tied to a specific game, programming language, company, model, or AI vendor. Its starting point is a portable extension to a project's AGENTS.md instructions, supported by a small set of living Markdown documents. Whether it later becomes a CLI, a skill, a plugin or something more substantial is an open design question.

## 2. The problem

AI-assisted development often happens in one increasingly long conversation. The conversation gradually becomes an unofficial combination of:

- Product vision, feature requirements and decisions.
- Technical constraints and discoveries.
- Implementation history, debugging attempts and test results.
- Personal preferences about workflow, code and design.
- Future feature ideas.
- Instructions for the current task.

This works until the context window fills, an agent reaches a usage limit, the user starts a different task, or another person or agent needs to take over. Context compaction can help, but a compressed conversation is not a dependable project knowledge base: it may omit reasoning, preserve superseded assumptions or mix speculation with agreed behavior.

Even when an assistant has memory across conversations, project-critical knowledge should be reviewable, versioned, portable and accessible to humans and other agents. It should not depend on a particular service remembering it correctly.

Traditional repository documentation is useful, but it often falls out of date because updating it is treated as a separate activity to be performed later. AI development increases the rate at which decisions are made, making that lag especially costly.

The problem is therefore not only storage or retrieval of context. It is **continuous maintenance of shared, authoritative project knowledge as part of the actual work**.

## 3. Product promise

A developer should be able to say:

> Read this repository, understand the product and its current state, and carry out this one task.

The developer should not have to explain the entire history again, find an old conversation, manually assemble a large handoff prompt, or keep using the same agent to preserve continuity.

A new product discussion should likewise be able to start from the repository, understand prior decisions and develop them further without accidentally reverting them.

For the owner, the experience should feel like working with a persistent development team, even though individual AI sessions are replaceable.

## 4. The collaboration model

The founding workflow separates responsibilities, not necessarily people or AI products. A single person or model can occupy multiple roles, but each session should have a clear purpose.

| Role | Responsibility |
| --- | --- |
| Human / project owner | Owns the product direction, grants authority, makes or approves consequential decisions and reviews the result. |
| Product Partner | Collaborates with the owner on product thinking, discovery, critical discussion, UX, design trade-offs, feasibility, and task definition. Maintains agreed context in the repository. |
| Developer Agent | Reads the current repository, understands the task and its reasons, implements an authorized change, tests it, and reports the result. |
| Reviewer / QA | Evaluates the change against the agreed specification, checks regressions and verifies what was actually tested. This can be the owner, the product partner, another agent, or a combination. |
| Repository | Carries the durable knowledge, code, decisions, history and collaboration artifacts between sessions. It is shared infrastructure, not an autonomous decision maker. |

In the original workflow the human and ChatGPT act like two product colleagues: they discuss the project at the level of product leads, refine ideas together, and turn those discussions into usable specifications. Codex acts primarily as the developer. ChatGPT also reviews implementation and helps investigate problems. The owner retains final authority; AI partners are not independent executives.

This division is valuable because product exploration and implementation have different context needs. The product partner needs the reasoning, unresolved questions and larger vision. A coding agent needs the relevant approved behavior, current code, constraints and a narrowly scoped objective. Both should read from the same source.

Evercanon must not require the exact ChatGPT/Codex combination. A human-only team, a different model, or a different editor or coding agent should be able to use the same repository.

## 5. The repository as the source of truth

The repository contains several kinds of truth that must not be conflated:

- **Product intent:** what the owner has agreed should be true.
- **Implemented behavior:** what the current code actually does.
- **Released behavior:** what users actually receive in a particular version.
- **Evidence:** what has been observed, measured, or verified, including limitations.
- **Exploration:** ideas that might be useful but are not approved implementation.
- **Working state:** unfinished tasks, branch/PR status, blockers and next actions.

The product specification may intentionally describe future behavior that is not yet shipped. The code may contain a known bug. A test may establish one scenario without proving all behavior. The system must preserve these distinctions rather than declare the newest document or the newest commit universally correct.

Version control gives the project an audit trail: what changed, why, when, and with whose authorization. The current accepted documents are the practical starting point; history is available through Git, PRs and linked decision records when needed.

The repository is authoritative for project knowledge, not a claim that every statement in it is automatically accurate. Agents must verify current facts and relevant code, flag contradictions, and reconcile them rather than silently choose whichever source is convenient.

## 6. Living documentation

The initial document model is inspired by the working process already used in Names in Red. Exact names and locations should remain configurable during product design. A candidate layout is:

```text
AGENTS.md
docs/
  PROJECT_CONTEXT.md
  PRODUCT_SPEC.md
  FUTURE_IDEAS.md
  TECHNICAL_VALIDATION.md
```

An existing project may already have PROJECT_CONTEXT.md at the root, a README, architecture documents, ADRs, an issue tracker or a different documentation convention. Evercanon should adapt to those assets instead of generating competing sources of truth.

### AGENTS.md — the entry point and working agreement

A small reusable addition tells compatible agents how to find the living documents, how to classify new knowledge, when to update it, and what authorization and review rules apply.

It should extend the project's existing instructions, not erase them. The general workflow belongs in Evercanon's reusable instructions; project-specific behavior belongs in each consuming repository.

### PROJECT_CONTEXT.md — a compact, current handoff

An entry point for a fresh session. It should explain the project's purpose, current state, architectural map, major constraints, meaningful decisions and rationale, relevant workflows, verified release status, unresolved issues and next steps.

It is a curated snapshot, not an append-only activity log or a compressed transcript of every conversation. Older details that remain useful can live in linked documents or Git history. Transient status should be checked against live repository state before acting.

### PRODUCT_SPEC.md — agreed product behavior

The current product goals, semantics, constraints, user experience, scope, edge cases and acceptance scenarios.

When a later agreed decision supersedes an earlier one, revise the earlier rule and reconcile affected examples and documents. Do not simply add a contradictory note to the bottom of the file. Distinguish target behavior from currently released behavior.

### FUTURE_IDEAS.md — optional exploration

A place to preserve promising ideas without turning them into obligations. An idea can be captured before every design detail is known, provided its core intention is understandable. Identify the unresolved questions and clearly label its exploratory status.

Do not discard ideas merely because they are outside the current release. Equally, do not let a coding agent interpret a brainstorm as an instruction to implement it.

### TECHNICAL_VALIDATION.md — evidence and constraints

A record of important technical research, experiments, regressions, known API limitations, test coverage and real-world observations. Link to source, relevant versions, fixtures, issues or PRs when possible.

Distinguish measured results from hypotheses, mocked behavior from live behavior, and an unverified approach from a confirmed solution. Avoid collecting raw logs, secrets, customer information or irrelevant personal data in the repository.

### Other existing documents

README, changelog, architecture documents, decision records, release notes and task trackers can retain their existing responsibilities. Evercanon should identify and update the appropriate document, not force every kind of knowledge into a single template or duplicate information already maintained elsewhere.

## 7. Continuous maintenance, not periodic context dumps

The defining behavior of Evercanon is that knowledge is maintained **while the conversation and work happen**.

When the product partner and owner settle a new product decision, meaningfully change the context, discover a relevant technical constraint, or articulate a future idea, the participating agent should update the appropriate repository document in the same working session. It should not wait until the user says "summarize our chat", until the context window is almost full, or until someone begins implementing the feature.

The working cycle:

1. Discuss the idea naturally with the owner. Explore alternatives and ask focused questions when an important ambiguity remains.
2. Recognize what has actually changed: an approved decision, optional idea, technical finding, or project-state update.
3. Read the relevant current repository documents and check for existing or conflicting statements.
4. Write a focused semantic update: capture the decision, necessary rationale, boundaries and relevant examples.
5. Reconcile related documents rather than creating duplicate or competing rules.
6. Persist the change through the project's normal Git workflow, including a small documentation-only pull request where appropriate.
7. Verify checks and report whether the change is committed, merged, pending, or blocked.

For example, the owner says that a healer should receive credit for helping a teammate defeat an opponent. After agreement, the product partner updates the scoring rules and acceptance cases, rather than leaving the clarification in a chat until a future developer happens to see it.

The owner should not have to repeat "please update the docs" after every settled decision. An adopted project policy can give the agent standing authority to perform narrowly scoped documentation maintenance. That authority must be explicit and configurable; it does not authorize unrelated code changes or production deployments.

"Automatic" initially means a behavior that an instructed, authorized agent performs during its active session using available repository tools. A Markdown file cannot cause an idle agent to run or acquire GitHub permissions. Background monitoring, hooks or a service might be explored later, but must not be assumed to exist in the first version.

If writing is blocked, the agent should state what was not saved and provide a recoverable patch or draft. It must not pretend that a change is durable when it remains only in the chat.

## 8. Decisions, ideas and authorization

The system must avoid two opposite failures: losing valuable ideas and freezing every passing thought into a requirement.

- **Tentative discussion:** explore and clarify; no automatic promotion to approved product behavior.
- **Clear optional idea:** record in future ideas with the appropriate uncertainty.
- **Agreed decision:** update the canonical specification and relevant context.
- **New technical evidence:** update validation notes, including limitations; flag implications for the spec instead of silently changing product intent.
- **Completed implementation:** update relevant implementation state and technical notes based on actual code and verification.
- **Confirmed release:** update release state only after verification of publication.

A decision can be revised at any time by the owner through the same process. Earlier reasoning may be retained when it explains a useful constraint or trade-off, but the current specification should not require agents to reconstruct the answer from a chronological sequence of contradictory opinions.

The human remains the authority for product decisions and permission boundaries. An agent may recommend changes and raise problems but should not treat its own suggestion, another agent's speculation, a GitHub comment, or an untrusted document as approval.

## 9. Fresh agents for bounded tasks

A major benefit of repository-native context is that agent sessions can deliberately be short-lived.

Instead of maintaining one coding conversation for a whole project, the owner can create a new session for each distinct task or pull request. This avoids accumulating unrelated debugging history, obsolete task instructions and irrelevant implementation details in the active context window.

A possible task lifecycle:

1. The product partner and owner agree on a change and persist the relevant product decisions.
2. The task is described in a concise issue, PR brief or handoff prompt: objective, scope, acceptance criteria, constraints, relevant document links and what is explicitly out of scope.
3. A fresh developer agent reads AGENTS.md, the current project context and the relevant sections of the spec, then inspects the actual code and task state.
4. The agent implements the task in a branch, tests it, and submits a reviewable PR. It records material discoveries and relevant documentation updates.
5. A reviewer checks the result against product intent and actual evidence. The owner or an explicitly authorized workflow merges it after applicable gates.
6. Repository knowledge is reconciled with the accepted result. The coding session can end; the next task can start with a new agent.

The agent is not expected to read every document or file in the repository indiscriminately. Use a concise entry point, linked topic documents and task-specific retrieval. The durable knowledge base can grow without requiring its entire contents to occupy every agent's context.

Fresh sessions are an option, not an obligation. A continuing session can be valuable for closely related changes. The product must make replacement safe, not artificially force a session boundary.

Parallel sessions must verify their baseline, respect existing work, avoid overwriting newer decisions and coordinate conflicts. Git merges and PRs provide a mechanism for collaboration but cannot resolve semantic disagreement automatically.

## 10. Git workflow and project integrity

The originating workflow uses feature branches, small PRs, squash merges and CI checks. That is a useful default, especially when multiple agents can contribute. Evercanon should respect the consuming project's actual policy rather than unilaterally reconfigure it.

- Prefer reviewable, narrowly scoped documentation changes when discussion changes the product contract.
- Record the rationale and scope in the PR.
- Check the latest target branch and any competing work before updating or merging.
- Respect branch protection, required checks, approval rules and repository access.
- When explicitly authorized, merge safe documentation-only PRs without repeatedly asking for permission.
- Keep implementation, infrastructure and releases separately authorized.
- Do not treat an absent CI configuration as passing tests. Report actual checks and verification.
- Never force-push or bypass project policy to maintain context.

A developer may update implementation-related documentation in the same code PR when that keeps behavior and documentation consistent. A separate documentation PR is especially useful when the discussion is independent of implementation. There is no need for a separate PR for every sentence; a single coherent decision is the useful unit.

## 11. Adoption and portability

The first version should be easy to adopt without committing to a particular runtime, package manager, hosting platform or programming language.

Desired experience:

- Connect Evercanon to a new or existing repository.
- Add or integrate its reusable instructions into AGENTS.md or an equivalent supported entry point.
- Initialize missing living documents with useful templates, based on existing project information where available.
- Preserve existing documents and user instructions; detect conflicts and ask only for consequential missing decisions.
- Make the repository usable by a fresh product or developer agent immediately.
- Continue maintaining the documents during ordinary discussion and task work.

The reusable framework should be versioned independently from consuming projects. Projects own their own canonical documents. Updating the shared workflow must not overwrite product content or silently expand agent permissions.

Potential distribution approaches include a copyable starter kit, pinned Git dependency, template installer, skill or CLI. These are candidates to evaluate, not architecture decisions. A project must be able to work with its repository even if the original Evercanon service or AI provider is unavailable.

## 12. Minimal viable product

The initial goal is to prove the workflow with the smallest useful system.

A plausible MVP consists of:

- A reusable AGENTS.md extension explaining the collaboration model, startup reading, decision classification and continuous maintenance.
- A small set of Markdown templates for project context, product spec, future ideas and technical evidence.
- A bootstrap procedure for new and existing repositories that preserves existing content.
- A documented product-discussion workflow and fresh-task-agent handoff protocol.
- A documented closeout and reconciliation workflow, including review and Git policy.
- Examples and acceptance scenarios based on a real project.

The exact document layout, installation mechanism and template structure should be designed after the founding concept is reviewed. A full CLI, centralized context database, automated multi-agent orchestration, web dashboard and model-specific integrations are not necessary to validate the concept.

The MVP is useful even if the only automation is agents faithfully following repository instructions. Software should be added when it solves an observed problem in that workflow, not because a framework is expected to have a complex runtime.

## 13. Example: how the idea emerged

The founding example is [Names in Red](https://github.com/nick-sidorin/names-in-red), a personal PvP journal addon. Its domain is incidental; the collaboration pattern is general.

During product discussion, the owner and ChatGPT recognized that the existing battle outcome model was trying to determine an objective group result, while the product should tell the local character's story. They agreed on three independent concepts: personal outcome, rival-specific participation credit, and observed group context.

The owner then clarified that participation credit must include meaningful healing and support, not just finishing blows. These changes were recorded in the product specification alongside acceptance scenarios, historical evidence and constraints, rather than left as chat-only instructions.

The project also gained a durable PROJECT_CONTEXT.md containing a compact handoff, technical limitations, previous decisions, release state and next work. Future ideas were kept separately, so an optional feature did not become part of the next implementation automatically.

Relevant public artifacts:
- [Product specification](https://github.com/nick-sidorin/names-in-red/blob/main/docs/skirmish-product-spec.md)
- [Project context](https://github.com/nick-sidorin/names-in-red/blob/main/PROJECT_CONTEXT.md)
- [Future ideas](https://github.com/nick-sidorin/names-in-red/blob/main/docs/future-ideas.md)
- [Character POV documentation PR #35](https://github.com/nick-sidorin/names-in-red/pull/35)
- [Continuous documentation working agreement PR #36](https://github.com/nick-sidorin/names-in-red/pull/36)

A separate practical example occurred during release: a coding session reached its usage limit, but another assistant was able to inspect repository state, resume the authorized release work and finish GitHub publication. This demonstrated the value of handoff through shared artifacts rather than a specific agent's availability. It also highlighted the need to distinguish actual confirmed publication from unverified external distribution.

The owner liked this collaboration so much that they wanted a reusable version for unrelated software projects. Evercanon is the result of that insight. It should generalize the process, not copy Warcraft-specific engineering rules.

## 14. Product principles

1. **Repository first:** durable knowledge belongs with the project, not only in a chat.
2. **Living, not archival:** current decisions are reconciled as they change; historical reasoning remains accessible without bloating the active context.
3. **Human-led collaboration:** AI can challenge and develop ideas, but authority stays with the human and project policies.
4. **Replaceable agents:** no essential knowledge should exist exclusively in one agent session.
5. **Separation of concerns:** product thinking, implementation, evidence, exploration and release state have different meanings.
6. **Minimal ceremony:** ordinary conversation should naturally produce focused updates; avoid mandatory forms or documentation churn for every message.
7. **Traceable and honest:** retain rationale, evidence, status and meaningful uncertainty; never claim an update or release that did not happen.
8. **Portable and incremental:** adopt in existing projects with their conventions; avoid provider lock-in and unnecessary infrastructure.
9. **Safe by design:** preserve permissions, confidentiality, repository policy and explicit authorization boundaries.
10. **Useful to humans too:** the documents must be readable, reviewable and valuable without an AI.

## 15. Success and acceptance scenarios

Evercanon is successful if these workflows become routine:

| Scenario | Desired result |
| --- | --- |
| Start a fresh product conversation | The partner reads the repository and understands the project without a long manual recap. |
| Agree on a changed feature | The specification is updated during the discussion, including conflicting old rules and affected acceptance cases. |
| Suggest a future enhancement | The idea is preserved without being implemented or promoted to an approved requirement. |
| Discover an API constraint | Evidence, exact scope and uncertainty are recorded so the next agent does not repeat the failed investigation. |
| Start a new coding task | A fresh agent can read the relevant context and perform the bounded task without unrelated chat history. |
| Interrupt an agent mid-task | Another agent can inspect branch, PR, tests and project state to resume without guessing. |
| Merge implementation | The knowledge base reflects what was accepted, distinguishing intended, implemented and released behavior. |
| Change an earlier decision | Current documents are reconciled; the old reasoning is available through history, not as a competing requirement. |
| Use an existing repository | Existing AGENTS.md and docs are preserved or deliberately integrated. |
| Change AI vendors | The shared Markdown and Git history remain useful. |
| Block a documentation write | The user is told that persistence failed and can recover the proposed change. |
| Revisit after months | A concise, current handoff and linked evidence explain the project without requiring old chats. |

Possible future measurements: time to onboard a fresh agent, frequency of owner re-explanation, repeated investigations, contradictory requirements, stale context, documentation-update latency and amount of task-irrelevant context loaded. No quantitative target is established yet.

## 16. Risks and open design questions

- How much structure is enough to guide agents without making documentation a second job?
- How should the initial installer detect and integrate existing documents instead of duplicating them?
- How can documents remain concise as the project grows? Should topic indexes, linked ADRs or optional compaction rules be introduced?
- What should be automatically checked (links, status markers, required sections, freshness) and what requires semantic review?
- How should competing decisions from parallel sessions be surfaced and reconciled?
- What is the best portable distribution/update mechanism for shared instructions?
- How should agents with different instruction formats or read-only repository access participate?
- How can agents detect meaningful decision changes reliably without misclassifying brainstorms as settled requirements?
- How should task state and handoff interact with existing issue trackers, PRs, local uncommitted changes and CI?
- How should sensitive work context be kept out of public repositories or restricted documents?
- What existing tools and methodologies overlap with this idea, and which problems are already solved well?

These are questions for future product discovery and technical design, not requirements to answer before preserving the idea. The system must not imply that natural-language instructions guarantee compliance by every agent. Tests, review and explicit status reporting remain necessary.

## 17. Possible later extensions

Explore only where the minimal workflow reveals a need:

- CLI initialization, upgrades, health checks and template management.
- Agent-readable document index and task-specific context packs.
- Documentation drift detection and checks for known contradictions or broken references.
- Handoff generators that build concise task briefs from canonical documents and live Git state.
- Integration with issue trackers, PR review, coding agents and CI.
- Optional role-specific instructions or skills for product partner, developer and reviewer.
- Consent-based, explicit orchestration of multiple agents or independent tasks.
- Support for larger repositories, multiple components, team-specific contexts and private documentation.
- Tools to reconcile or compact growing documents without losing approved decisions and provenance.

Evercanon is not initially a replacement for Git, GitHub, project management software, a coding agent, or AI context-window management. It is a collaboration and knowledge-maintenance layer that can complement them.

## 18. Next steps

The current repository is only the home of this founding vision. No software has been built and no architecture is selected.

Continue in a separate conversation devoted to Evercanon:

1. Read this document and confirm the desired workflow and initial target users (starting with the founder's own projects).
2. Research existing repository-native context and agent-instruction tools to identify overlap and useful conventions.
3. Define a small product specification, including the document taxonomy, authority rules and the initial bootstrap experience.
4. Decide how to distribute the first reusable instruction/template kit and how to test it on real repositories.
5. Develop Evercanon using the same living-documentation methodology wherever practical. It should become an early user of its own approach.

Suggested new-session prompt:

> We are designing Evercanon, a repository-native framework for human–AI collaboration. Read PRODUCT_VISION.md in https://github.com/nick-sidorin/evercanon. It captures the founding discussion and the workflow that motivated the project. Continue from that vision; help me refine the product and evaluate existing approaches before choosing a technical implementation. Do not assume the proposed MVP architecture has already been selected.

---

**The guiding principle: the project should remember, even when the agent does not.**
