# SYSTEM_ARCHITECTURE.md

## Mission

Build a durable, governed, AI-driven automation system capable of: -
Managing Monday.com outreach and CRM workflows - Persisting canonical
memory in Postgres - Executing UI-based setup and configuration
autonomously - Operating independently of ChatGPT web session continuity

------------------------------------------------------------------------

## Current Architecture (2026-03-02)

**Brain Layer** - OpenAI API (LLM via API --- not ChatGPT web UI)

**Agent Runtime** - OpenClaw Gateway (deployed on Render)

**Deterministic Automation Spine** - n8n (Render-hosted)

**Memory / Source of Truth** - Render Postgres database

**Primary Business UI** - Monday.com

**Messaging / Comms** - Gmail - Slack (optional)

------------------------------------------------------------------------

## Role of Each Component

### OpenAI API (Brain)

Provides reasoning, planning, architectural decision support, and
execution logic.

### OpenClaw (Runtime)

Persistent agent environment. Provides: - Browser automation - Tool
execution - Autonomous task loops - Session persistence - UI interaction
and verification

### n8n

Handles deterministic workflows: - Webhook ingestion - Simple automation
routing - API-triggered actions - Scheduled tasks

### Postgres

Canonical long-term memory. Stores: - Event logs - System state - IDs
for CRM objects - Audit trail No state lives only in UI.

### Monday.com

Human-facing CRM interface. Source of business workflow state but not
canonical system memory.

------------------------------------------------------------------------

## Data Flow Overview

Monday → n8n webhook → Postgres (event logged) OpenClaw → UI action →
Monday OpenClaw → write audit → Postgres n8n → API-based updates →
Monday

------------------------------------------------------------------------

## Non-Negotiable Rules

1.  Postgres is canonical memory.
2.  No duplicate execution mechanisms.
3.  UI automation is not the source of truth.
4.  All major architectural changes must be recorded in DECISION_LOG.md.
5.  No dependency on ChatGPT web session continuity.
