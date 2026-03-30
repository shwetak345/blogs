---
layout: post
title: "Why Your LLM Pipeline is Failing (and How to Fix It)"
date: 2026-03-29
categories: ai
excerpt: "Why LLM pipelines fail in production and how to fix them using structured evaluation."
---

![LLM Pipeline]({{ site.baseurl }}/assets/images/llm-eval-lifecycle-diagram.png)

# Why Your LLM Pipeline is Failing (and How to Fix It)

We’ve all been there: you build a sleek LLM-powered application, it works perfectly in your first three tests, and then—total chaos in production. The "vibes" are suddenly off, or worse, the model is confidently hallucinating. 

The hard truth? While traditional tests like unit and integration tests can tell you if your code runs, and F1 scores can tell you if your classifier is accurate, they aren't enough for the non-deterministic world of LLMs. To build reliable AI, you need to bridge the Three Gulfs of LLM Development: The Gulf of Comprehension, The Gulf of Specification and The Gulf of Generalization.

## 1. The Gulf of Comprehension

At scale, you can’t manually read every input or output. Without a systematic way to see patterns in your data, you’re flying blind. 

- The Fix: Start with "Open Coding." Read 30–50 traces and label exactly where they fail. Don't guess; look at the data. 

## 2. The Gulf of Specification

What you mean and what you prompt are rarely the same. "Summarize this email" is too vague. Does the user want a paragraph or bullets? Without complete instructions, the model is forced to “guess” often resulting in inconsistent outputs. The Gulf of Specification captures this gap between what we want and what we actually communicate.

- The Fix: Be explicit about personas and response rules (e.g., "The summary must be exactly three sentences")

## 3. The Gulf of Generalization

The third gulf separates our data from the pipeline’s generalization behavior. Even if prompts are carefully written, LLMs may behave inconsistently across different inputs when encountering new or unusual inputs. A model might correctly extract names until it hits an email mentioning a celebrity and gets confused. 

- The Fix: You need targeted, application-centric evals—not just generic benchmarks like MMLU.

## The Path Forward
Start measuring.
Next, I’ll dive into the **Analyze–Measure–Improve** lifecycle that the best AI teams use to ship with confidence.

## The "Analyze-Measure-Improve" Flywheel for AI Engineers

If you want to move beyond "vibes-based" development, you need a repeatable lifecycle. Here’s a practical framework for application-centric AI evaluation:

### Step 1: Analyze (Qualitative)

Before you can measure success, you must first understand failure. This phase is about bridging the Gulf of Comprehension through a structured three-step process:

#### 1. Create a Representative Trace Dataset

You cannot improve what you cannot see. A "trace" is the full record of an LLM’s journey - from the initial user prompt and tool calls to RAG and the final output.

- Source your data: Use production logs to sample traces across various use cases.
- The "Cold-Start" Solution (Synthetic Query Generation): If you don’t have production data yet, use a "Teacher LLM" to generate diverse, grounded prompts. By defining specific user personas (e.g., "a first-time homebuyer") and injecting your domain data, you can simulate a realistic test suite before your first real user ever logs in.

#### 2. Identify and Isolate Failure Modes (Open & Axial Coding)

Don't just label a response as "wrong." You must isolate exactly where the chain broke—was it the model's "brain" or the infrastructure supporting it?

- Open Coding: Review 30–50 traces and write free-form, informal notes. Don't worry about consistency yet—just capture every "vibe check" failure or technical glitch.
- Axial Coding (The Isolation Phase): Group those messy notes into a structured taxonomy. The goal is to categorize these into actionable buckets for your engineering team:

    - Retrieval (RAG) Issues: Did the retrieval pipeline fetch the wrong documents? Was meaningful information buried too low in the results, or was the context window flooded with noisy, irrelevant data?
    - Tool-Calling Issues: Did the model choose the wrong tool, generate invalid arguments for the tool, or fail to understand the tool's intended use?
    - Infrastructure Issues: This is the "silent killer." Did the API call succeed technically but return zero results? Did the system time out or fail to parse a complex PDF/image?
    - Model-Specific Issues: Did the model have all the right information but simply failed to follow the persona, ignore a constraint, or use the wrong tone?

#### 3. Structured Labeling & Alignment

Once you have your list of failure modes, you must go back and systematically re-label your dataset.

- The Binary Check: For every trace, mark a 1 (Present) or 0 (Absent) for each failure mode. This turns subjective "feelings" into a structured table that you can actually count and track.
- Collaborative Alignment: Use these labeled traces in "alignment sessions" with your team. If two engineers disagree on whether a trace is a "Tone Violation," you haven’t defined your "Pass/Fail" criteria clearly enough. Use these sessions to lock down your definitions.

Trace ID | Constraint Violation | Hallucinated Facts | Persona Mismatch | Tone Violation | Overall Outcome  
--- | --- | --- | --- | --- | ---  
T1 | 1 | 0 | 0 | 1 | FAIL  
T2 | 0 | 1 | 0 | 0 | FAIL  
T3 | 0 | 0 | 0 | 0 | PASS  

#### Why this matters:

This level of rigor enables you to move from guessing to counting. You can now prioritize fixes based on which failures are most frequent and track how your metrics improve over time as you iterate on your prompts and architecture. This structure provides the foundation for the next stage: Measure.

### Step 2: Measure (Quantitative)

Once you’ve identified how your system fails, you need to turn those qualitative insights into repeatable numbers. This phase moves you from "vibe checks" to engineering rigor.

#### 1. Selecting the Right Evaluator (Code vs. LLM-as-Judge)

Once you have your list of failure modes, you must decide how to detect them automatically.

- Code-Based Evaluators (Deterministic): Use these for objective failures. If a failure mode is "Invalid JSON," "SQL Syntax Error," or "Output exceeds 500 words," use a Python script. It’s 100% accurate, fast, and free.
- LLM-as-Judge (Heuristic): Use these for failure modes that require "human-like" nuance. You cannot write a regex for "Is this response polite?" or "Is this summary faithful to the original text?" For these, you prompt a highly capable model (the Judge) to evaluate the output of your application model.

#### 2. Define Rubrics and Align with Kappa Scores

To make an LLM-as-Judge reliable, you need a Rubric (a clear set of instructions defining what constitutes a "Pass" or "Fail").

- Calibration: Use the same traces from your Analyze phase to test your rubric. If a human labeled a trace as a "Failure" but the Judge calls it a "Pass," your rubric needs more detail.
- The Kappa Score: Before automating, ensure your human experts actually agree. The Kappa Score measures "Inter-Annotator Agreement." If your Kappa score is low, your failure definitions are too vague for any Judge (human or AI) to measure consistently.

#### 3. Creating the "Golden Dataset"

A Golden Dataset is a small, high-quality collection of prompts and "perfect" responses (or labeled failures) that have been verified by humans.

- How it's created: You curate it from your best Traces. You pick the most representative examples of "Perfect Success" and "Clear Failure" for each category.
- Purpose: This becomes your "Ground Truth." Every time you change your prompt or model, you run it against this Golden Dataset. If your "Golden" examples start failing, you know you've had a regression.

#### 4. Evaluating the Judge (TPR, TNR, and Success)

You must treat your Judge like any other ML model. You need to know if the Judge itself is accurate.

- TPR (True Positive Rate): How often real Pass cases are judged as Pass by the Judge.
- TNR (True Negative Rate): How often real Fail cases are judged as Fail by the Judge.
- True Success Rate: The True Success Rate is the estimated percentage of production outputs that would be judged as successful by humans, after correcting the automated judge’s results for its known error rates (TPR and TNR).
- Confidence Interval: The Confidence Interval is a range around the estimated success rate that reflects uncertainty due to limited labeled data and imperfect evaluators. It indicates where the true success rate is likely to lie (e.g., 95% confidence), helping assess how reliable the estimate is. It is a measure of reliability. A narrow interval (e.g., ± 1%) means you are very certain of your results; a wide interval (e.g., ± 10%) means you need more labeled data before you can trust the number.

### Step 3: Improve & Deploy (The Continuous Reliability Loop)

The work doesn’t end at deployment. In the world of non-deterministic AI, Continuous Deployment (CD) means building a feedback loop that catches the "unknown unknowns."

#### 1. Close the Loop with Targeted Fixes

Because you isolated the root cause in Step 1 (Analyze), your improvements are now surgical rather than experimental:

- If it’s a Retrieval (RAG) Issue: Don't touch the prompt. Instead, refine your Retrieval Strategy—adjust chunking sizes, improve your re-ranking logic, or filter out noisy data.
- If it’s a Tool-Calling Issue: Refine your API documentation or parameter definitions. Often, the model just needs a clearer "instruction manual" for the tool.
- If it’s a Model Issue: This is where you iterate on the prompt, add few-shot examples to the context, or consider fine-tuning.

#### 2. Implement Real-Time Guardrails

While Evals measure quality after the fact, Guardrails protect the user during the interaction.

- Input Guardrails: Intercept toxic prompts, PII, or irrelevant queries before they reach your LLM to save on costs and prevent "jailbreaking."
- Output Guardrails: Use high-speed programmatic checks to catch "hallucination triggers" or broken formatting (like truncated JSON) before the user ever sees them.

If your guardrails are firing frequently, don't just celebrate that they "caught" the error—use those hits as a signal to go back to Step 1 (Analyze) and fix the root cause in your core pipeline.

#### 3. Maintain the "Thermometer": The Weekly Re-Calibration

Your automated Judge is only as good as its last alignment. LLMs drift, and user behavior evolves. To maintain a "True North" metric, you must re-calculate your Judge’s Accuracy (TPR/TNR) regularly.

The Golden Cadence:

- The Weekly Cadence: Every week—and on every new model update or application version—sample fresh production traces.
- Manual Re-Labeling: Have humans manually label this small sample and re-calculate the Judge's Accuracy (TPR, TNR) and Confidence Intervals.
- The Safety Net Expansion: When you find a new production failure, add it to your Golden Dataset. This ensures your CI safety net grows stronger as your product matures.

**References & Credits**

The concepts and frameworks discussed in this post — including the Three Gulfs and evaluation strategies — are inspired by the *AI Evals for Engineers and PMs* training by Hamel Husain and Shreya Shankar. Their work provides a strong foundation for building reliable, production-ready AI systems.