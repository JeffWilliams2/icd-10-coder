# ICD-10 Coder

> Explainable medical coding with RAG. Every suggested code links back to the source text that justifies it.

**Status:** Planned — Phase 4 of build plan

Open-source engine for explainable ICD-10 medical coding. Demonstrates RAG over the ICD-10-CM code set with constrained generation and source-span citation.

**Live demo:** *coming soon*
**Commercial version:** *coming soon*
**Built on:** [genai-rag-template](x)

## The problem

Generic LLMs can suggest medical codes but can't justify them. Coders won't trust suggestions they can't audit. Billing teams can't submit codes they can't trace. This project addresses that with explainability built into the architecture.

## How it works

1. The full ICD-10-CM code set (~70k codes) is pre-embedded in pgvector with official descriptions
2. A clinical note is split into clinical statements
3. For each statement, the top-20 semantically similar codes are retrieved
4. Claude is given the statement plus candidate codes and returns ranked codes with the **exact substring of source text** justifying each one

The constraint is the key: limiting Claude to retrieved candidates and requiring span citations means the model can't hallucinate codes that don't exist or invent justifications.

## Features in this repo

- [ ] ICD-10 code embedding pipeline
- [ ] Statement-level chunking of clinical notes
- [ ] Constrained generation with span citation
- [ ] Demo UI for pasting notes and viewing suggestions
- [ ] Eval suite against synthetic golden dataset

## Features in the commercial version (NOT in this repo)

- Multi-user auth and team workspaces
- Stripe billing and case management
- Audit logs for compliance
- Multi-agent workflow (triage, coding, reconciliation, confidence check)
- CPT code support
- BAA-eligible infrastructure

## Stack

Next.js, FastAPI, pgvector, Claude Sonnet. See the [template repo](x) for shared infrastructure.

## Important: Data handling and disclaimers

**This is a demo project. Do not use real patient data.**

The hosted demo accepts only synthetic clinical notes generated for portfolio purposes. The system is **not** HIPAA-compliant, does **not** offer a Business Associate Agreement (BAA), and has **not** been audited for use with Protected Health Information (PHI).

If you paste a note into the demo:
- It will be sent to third-party LLM APIs (Anthropic) for processing
- It may be retained in application logs for debugging
- It is not encrypted at rest with healthcare-grade controls
- The author assumes zero liability for any data submitted

**By using this demo, you affirm that any text you submit contains no real patient information, no PHI as defined under HIPAA, and no data that would require a BAA to process.** Synthetic notes are included in the repo — use those.

For real clinical use, see the commercial product, which is built with appropriate safeguards including encryption at rest, BAA availability for Team and Enterprise tiers, audit logging, and a reviewed compliance posture.

This tool provides coding suggestions only. All codes — even from the commercial version — must be reviewed by a certified coder before submission. Not a substitute for professional medical coding judgment.

## License

MIT
