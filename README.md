# n8n-orchestrator-ai

<img width="1748" height="616" alt="image" src="https://github.com/user-attachments/assets/feea29a7-5149-4f84-acf3-67dd866f41c5" />

# AI-Powered Transcription Orchestrator: Multi-Agent Map-Reduce Workflow

## 📌 Overview
This project is a high-efficiency automation pipeline built in n8n to process and summarize long Microsoft Teams transcriptions (.vtt files). Using a Map-Reduce architecture, the workflow overcomes LLM context window limitations by fragmenting data, processing it through localized models, and re-synthesizing it into a coherent global summary.

The entire stack is designed for privacy and data sovereignty, utilizing self-hosted Ollama models.

## 🏗️ Architecture: The "Map-Reduce" Strategy
Summarizing long meetings directly often leads to "hallucinations" or loss of detail due to token limits. This workflow solves it in three phases:

Preparation (Etl): Cleaning raw VTT data (stripping timestamps/headers) and batching it into manageable chunks.

Mapping (Chunk Resume): A fast, lightweight model (Llama 3.1 8b) summarizes each chunk in parallel to extract key points.

Reducing (Global Synthesis): A more powerful model (Mistral-Nemo) ingests the intermediate summaries to produce a final, structured report.

## 🛠️ Tech Stack
Orchestration: n8n (Self-hosted)

AI Engine: Ollama (Local)

Models: Llama 3.1:8b (Efficiency) & Mistral-Nemo (Reasoning)

Logic: Node.js / JavaScript (Custom Data Cleaning)

Delivery: Telegram API / Local File System

## 🚀 Workflow Breakdown
1. Ingestion & Pre-processing

On Form Submission: User uploads a .vtt file via a web form.

Clean File (JavaScript): A custom Node.js node sanitizes the text, removing noise like "WEBVTT" headers, timestamps, and redundant metadata.

Split in Batches: The text is divided into segments of 10,000 characters to ensure optimal token density for the local LLM.

2. Intelligent Loop

Loop Over Items: Iterates through each chunk.

AI Chunk Resume (Llama 3.1:8b): Performs a high-speed extraction of the most relevant information from each segment.

3. Final Consolidation

Concatenate Chunks (JavaScript): Re-assembles the summarized segments into a single context block.

AI All Resume (Mistral-Nemo): Generates the final executive summary. This model is chosen for its superior ability to maintain global coherence and professional tone.

Delivery: Sends the final result directly to a Telegram bot for immediate mobile access.

## 🔒 Privacy & Sovereignty
Built for enterprise environments where data privacy is non-negotiable:

GDPR Compliant: No data is sent to external cloud providers (OpenAI/Anthropic).

On-Premise: Everything runs on local infrastructure via Docker and Ollama.

Resource Optimized: Strategic use of small vs. large models to balance energy consumption and hardware performance.
