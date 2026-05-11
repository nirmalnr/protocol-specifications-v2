# Contributing to Beckn Protocol

**Scope:** This document applies to all repositories governed by the Beckn Core Working Group — `protocol-specifications-v2`, `schemas`, `DEG`, and any derivative protocol repositories.

---

## Read this before you begin

The Beckn Protocol contribution process is intentionally designed to be difficult. This is not an oversight. It is a deliberate architectural decision made to protect the ecosystem.

Consider how other foundational protocols work. HTTP does NOT add a new method because a new social media platform was launched. TCP/IP did NOT change because video streaming became popular. The HDMI protocol does NOT introduce a new connector because a new television manufacturer enters the market. These protocols are stable because their designers understood that the cost of a protocol change is borne not by the person proposing the change but by every implementation that depends on it — often across organizations, jurisdictions, and years.

Beckn Protocol is the same. A change to the core protocol or its schemas affects every network operator, every platform builder, and every downstream implementer in the ecosystem — most of whom you have never met and who had no say in your proposal. The governance model is designed to force contributors to confront this reality before committing engineering effort.

**Before opening any Issue, Discussion, or PR, ask this question honestly:** does the Beckn Protocol need to change, or does something at a higher layer need to change?

The Beckn ecosystem is a layered stack. Changes MUST be attempted at the outermost layer first and escalate inward only when genuinely necessary. Work through this sequence before proposing any change to a governed repository:

1. **On-Fabric implementations** — Can this be resolved as a network policy, operator configuration, or application-layer change? If yes, it does NOT belong in this repository.
2. **Adapter specs** — Can this be handled by an adapter layer such as ONIX? If yes, it does NOT belong in this repository.
3. **Schemas** — Can this be addressed by a new or extended schema in `beckn/schemas` without touching transport semantics? If yes, it does NOT belong in `protocol-specifications-v2`.
4. **In-Fabric stateful protocols** — Can this be addressed by a new or modified Fabric service API? If yes, it does NOT belong in the core protocol.
5. **Core P2P stateless protocol** — Only if none of the above layers can absorb the requirement does a change to `beckn.yaml` belong here. This is the last resort, not the first.

If you reach step 5 with honest answers to steps 1 through 4, the process described below is how you make that case.

Before proceeding further, read all of the following in full:

- [GOVERNANCE.md](./GOVERNANCE.md) — the governance model, the ecosystem stack, and why this process is the way it is.
- [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md) — the conduct standards and enforcement ladder.
- [NFH-001 — Architecture and Design Principles](./docs/Introduction.md) — the seven design principles every change must satisfy.
- [NFH-005 — Specification Design Guide](./docs/Design_Guide.md) — the authoring standards for all specification artifacts.
- [NFH-006 — RFC Writing Guide](./docs/NFH-006_RFC_Writing_Guide.md) — the RFC template and quality standard.

These documents are not background reading. They are prerequisites. A contribution that does not reflect their requirements will be closed.

---

## A note on when these rules are binding

These governance documents are formally binding from the moment they are merged to the `main` branch. Until that merge occurs, they exist on the `draft` branch as a work in progress.

Draft status does NOT create permission for intentional violations during the drafting period. Contributors are expected to act in good faith at all times, regardless of the formal enforcement status of any specific rule. For the full statement of this principle, see the "When these rules apply" section in [GOVERNANCE.md](./GOVERNANCE.md).

---

## Registry protocols

Registry protocols used within the Beckn ecosystem follow the DeDi (Decentralized Identity) protocol specifications hosted under the Linux Foundation Decentralized Trust initiative. Those protocols are governed by Linux Foundation Decentralized Trust, NOT by this governance model. Changes to registry protocols MUST be proposed through the Linux Foundation Decentralized Trust process, not through this repository.

---

## Contribution principles

Every contribution to any governed repository MUST satisfy all seven design principles defined in [NFH-001](./docs/Introduction.md):

1. **Decentralization** — the change increases, NOT reduces, agency at the network edges.
2. **Fabric-driven** — the change emerges from a documented Fabric capability requirement, NOT from implementation convenience or platform preference.
3. **Agent-first** — the proposed change MUST be exercisable by an AI Agent without human intervention at any protocol step.
4. **Pragmatism** — the practical implications on non-AI-native systems and current ecosystem tooling are acknowledged.
5. **Semantic interoperability** — all terms are defined, unambiguous, and interpretable consistently across domains, regions, and implementations.
6. **Reusability via abstraction** — existing schemas at [schema.beckn.io](https://schema.beckn.io) and existing endpoints MUST be surveyed and found insufficient before new ones are proposed.
7. **Trust by design** — signature and acknowledgement requirements MUST remain baseline transport behavior.

A contribution that cannot affirmatively satisfy all seven principles is NOT ready for the RFC process, regardless of its technical quality.

---

## The RFC lifecycle

Every protocol change follows this sequence. Each stage is a prerequisite for the next. No stage may be skipped. Submitting a PR before the preceding stage is complete results in immediate closure without review.

### Stage 1 — Idea

Post a Discussion in the `Ideas` category of the relevant repository. No formal structure is required. Describe the problem you are observing and why you believe it may require a protocol change at the layer you are targeting. Invite working group and community feedback.

An Idea that does NOT generate working group engagement is a signal that the problem may not require a protocol change, or may be solvable at a higher layer in the stack. Proceed to Stage 2 only if there is genuine working group interest.

### Stage 2 — Proposal

Open a Discussion in the `Proposals` category. The Proposal MUST:

- Be authored using the [NFH-006 RFC template](./docs/NFH-006_RFC_Writing_Guide.md) in full.
- Link to the Idea Discussion from Stage 1.
- Include all mandatory RFC sections: Abstract, Motivation (with a traceable Fabric requirement), Design Goals and Non-Goals, Roles and Actors, Protocol Flows with sequence diagrams for all async flows, Schema Changes, Security Considerations, Privacy Considerations, Prior Art.
- Pass the NFH-006 Pre-Submission Checklist (Appendix B) before posting.
- Demonstrate in the Motivation section why the change cannot be addressed at a higher layer in the ecosystem stack.

The Proposal is subject to working group review. Working group consensus is required to advance. The working group may request revisions, reject the Proposal, or defer it to a future review cycle. A rejected Proposal does NOT automatically become eligible for resubmission — the contributor MUST address the stated concerns before reopening.

**Do NOT submit a Pull Request until the Proposal has reached working group consensus.** A PR submitted before Proposal approval is closed without review (CON-005-18).

### Stage 3 — Protocol Draft (PR to `draft` branch)

Once the Proposal is approved, submit a PR to the `draft` branch. The PR MUST:

- Link to the approved Proposal Discussion.
- Contain ONLY the artifacts described in the approved Proposal. Changes outside the Proposal scope result in immediate closure (CON-005-19).
- Pass the RFC Approval Gate in [NFH-005](./docs/Design_Guide.md): the linked Proposal MUST be at working group-approved status.
- Pass all NFH-005 conformance checks (CON-005-01 through CON-005-19), as reviewed by a Core Committer.
- Include a companion PR to the `schemas` repository if the change introduces new named terms in `beckn.yaml`.
- Set the RFC `ID` field to `NFH-TBD`. The actual `NFH-NNN` identifier is assigned automatically by a GitHub Action on merge to `main` — it is NOT assigned at this stage.

Upon merge to `draft`, the RFC reaches Protocol Draft status. The contributor's active involvement in the process ends here. What happens next is managed by the Core Working Group through the release process.

### Stage 4 — Protocol Standard (release management)

Protocol Standard progression is managed entirely by the Core Working Group. Contributors do NOT initiate or control this stage. When the working group determines that a set of Protocol Drafts is ready for release, the Working Group Administrator manages the release process defined in the "Branch and release management" section of [GOVERNANCE.md](./GOVERNANCE.md).

For awareness, the release process involves:

- A release branch cut from `main` in the format `core-v2.x.y-lts-rcX`.
- Cherry-picking of scheduled Protocol Draft changes from `draft` onto the release branch.
- A staging PR to the release branch. Before this PR is merged, the contributor who opens it MUST explicitly confirm their agreement to the Contributor License Agreement (CLA) in the PR thread.
- A PR from the release branch to `main`, reviewed by at least three CWG members including the Chief Architect, with a passing conformance regression test required before any approvals are recorded.
- On merge to `main`: automatic RFC ID assignment via GitHub Action across all NFH repositories; release tag decision by the working group; announcement published in Discussions within 24 hours.

**Protocol version and release version are NOT the same thing.** The release tag (e.g., `core-v2.1.0-lts`) is a governance label identifying a published state of the specification. It is NOT the value of `context.version` in `beckn.yaml`. The `context.version` field used by conformant implementations at runtime remains `2.0.0` until explicitly changed in `beckn.yaml` by the working group. A new release tag does NOT mean the protocol version has changed.

Once merged to `main`, the RFC is assigned one of three Protocol Standard types: `Protocol Standard - REQUIRED`, `Protocol Standard - RECOMMENDED`, or `Protocol Standard - NOT RECOMMENDED`.

---

## Issues

Issues in governed repositories are Bug or Enhancement records linked to the release pipeline. They are NOT feature request trackers or discussion proxies. An Issue that does NOT link to an approved Proposal Discussion will be closed.

Issues of type Bug or Enhancement are required as linked artifacts when a change progresses to the `main` branch (Protocol Standard stage). They serve as the human-readable record of what production problem or ecosystem need the change addresses.

Every Issue MUST include:

| Field | Description |
|---|---|
| **Title** | Clear, implementation-oriented summary |
| **Type** | `Bug` or `Enhancement` |
| **Problem statement** | What is broken or missing, with evidence |
| **Affected layer** | Which layer in the ecosystem stack this Issue targets |
| **Scope** | `api`, `schema`, `json-ld`, or combination |
| **Affected repositories** | All governed repositories affected |
| **Current behavior** | What conformant implementations currently do |
| **Expected behavior** | What conformant implementations should do |
| **Conformance impact** | `hotfix`, `patch`, `minor`, or `major` |
| **Backward compatibility** | Compatible or breaking; if breaking, migration path |
| **Security/privacy impact** | Explicit statement — never left blank |
| **Linked Proposal Discussion** | URL to the approved Proposal |

---

## PR quality checklist

Before requesting review on a Protocol Draft PR, confirm all of the following. A PR that cannot satisfy this checklist MUST remain in Draft status.

**Process and RFC gate**
- [ ] Idea Discussion exists in the `Ideas` category and is linked from the Proposal
- [ ] Proposal Discussion is in the `Proposals` category, approved by the working group, and linked from this PR
- [ ] PR targets `draft`, NOT `main`
- [ ] PR diff is fully within the scope of the approved Proposal (CON-005-19)
- [ ] RFC `ID` field is `NFH-TBD`

**Stack layer validation**
- [ ] The Motivation section demonstrates why the change cannot be addressed at a higher layer (on-Fabric implementations → adapter specs → schemas → in-Fabric stateful protocols) before reaching the core protocol

**Schema and artifact conformance**
- [ ] All new schema properties use `lowerCamelCase` (CON-005-01)
- [ ] All enum values use `SCREAMING_SNAKE_CASE` (CON-005-02)
- [ ] All new schemas are in the `schemas` repository with `context.jsonld`, `vocab.jsonld`, `attributes.yaml`, `attributes.jsonschema.yaml`, and `README.md` (CON-005-13, CON-005-14, CON-005-15)
- [ ] Breaking renames include `owl:deprecated` and `owl:sameAs` entries in `vocab.jsonld` (CON-005-04)
- [ ] All examples validate against the schema (`npx @redocly/cli lint`) (CON-005-05)
- [ ] Stateful API endpoints use noun paths, REST HTTP verbs, and path parameters for resource identifiers (CON-005-12, CON-005-16)
- [ ] Stateful async callbacks use the same resource path on the caller's registered URI — no `on_*` pattern on stateful APIs (CON-005-17)
- [ ] Registry protocol changes are NOT included — those belong in Linux Foundation Decentralized Trust governance
- [ ] No BG (Beckn Gateway) references appear anywhere in the diff — BG is a v1.0-only concept

**RFC quality**
- [ ] NFH-006 Pre-Submission Checklist (Appendix B) completed in full
- [ ] Security Considerations drafted before Protocol Flows, per NFH-006
- [ ] All normative statements use MUST / MUST NOT / SHOULD / SHOULD NOT / MAY
- [ ] All async flows have sequence diagrams
- [ ] All lifecycle resources have state machine diagrams

---

## Conduct expectations

All contribution activity is subject to [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md). The conduct standards apply equally to Discussions, Issues, PRs, and working group meetings.

The enforcement ladder for violations is:

| Stage | Trigger | Action |
|---|---|---|
| **Notice** | First documented violation | PR, Issue, or Discussion closed with documented reason. Contributor notified. |
| **Suspension** | Repeated violations after notice | PR submission privileges suspended. |
| **Restriction** | Continued violations after suspension | Issue and Discussion access restricted. |
| **Removal** | Severe or persistent violations | Contributor removed from the organization across all governed repositories. |

A removed contributor who wishes to be reinstated MUST submit a formal written request to the Working Group Administrator identifying the root cause of the violation and the remediation taken. The Core Working Group reserves the right to accept or deny such requests. For the full reinstatement process, see [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md).

---

## What happens if you bypass this process

A PR submitted without a working group-approved Proposal Discussion is closed without review. A PR whose diff exceeds the approved Proposal scope is closed without review. These closures are automatic. They are NOT a judgment about engineering quality.

Repeated bypass attempts are conduct violations, not technical infractions. They are treated under [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md) and result in the enforcement ladder above.

If you believe this process is wrong, the correct response is to propose a change to the governance model through the RFC process — starting with an Idea Discussion in the `Ideas` category. The correct response is NOT to bypass the process on the grounds that your change is urgent, obviously correct, or has significant ecosystem support. No contribution is entitled to bypass the RFC lifecycle.
