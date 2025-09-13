---
title: I “Vibe-Coded” You a Management System for 100,500 Jupyter Notebooks
tags: juport jupyter reports python
---

I’ll tell the story of how I solved the chaos in my Jupyter reports and built **juport (Jupyter Report System)** [GitHub link](https://github.com/tumurzakov/juport). I’ll also share a few thoughts on how development is changing in the era of AI assistants.

## The Problem: 100,500 Reports and No Order

I had accumulated a huge number of reports made in Jupyter Lab. Each one was a separate file with code, passwords, and all the behind-the-scenes “kitchen.”

**Main pain points:**

* **Security.** You can’t just share a report with management or accounting because it contains database access and code.
* **Routine.** There’s no centralized place to run reports, set up schedules, and have a unified viewing interface.
* **Chaos.** All results are scattered across folders, and finding the right Excel file took a lot of digging.

## The Concept

I needed something that would let me develop reports in the familiar Jupyter Lab, then automatically run them, generate clean HTML versions without code, and gather all artifacts in one place.

### The Solution: **juport** — a report management system

I created a system (well, more like vibe-coded it) consisting of two components:

1. **Jupyter Lab** sidecar. A standard Jupyter Lab in a Docker container. This is where developers write and test reports as usual.
2. **juport** **— the management system.** A Python web app that scans the notebook folder. It lets you run reports manually or on a schedule, executes them in an isolated environment, generates HTML versions stripped of extra info, collects all artifacts (Excel, images) into a single table, and provides a convenient web interface. Authentication is via **LDAP**.

## How It Works

**Report development:**

1. You create a notebook in Jupyter Lab.
2. You write code, test it, save it.
3. You use environment variables for configuration so passwords aren’t stored in code.

**Report execution:**

1. You open the **juport** web interface.
2. You see a list of all notebooks.
3. You click “Run” or set up a schedule.
4. The system executes the notebook and collects results.

**Outcome:**

* A clean HTML report without code or passwords, available for viewing.
* All Excel files, images, and PDFs collected in one place.
* A convenient download interface.
* Execution history and logs.

---

## How It Was Built

I didn’t write a single line of code myself. I **vibe-coded** everything via **Cursor** using prompts.

Yes, exactly. Get used to it. That’s the reality.

Andrej Karpathy said that development will look very different soon. He’s right.

We, **millennials**, are the only generation that knew how to build a computer from scratch. Boomers were before the PC boom, and zoomers were born when everything was already set. The same is happening with code. In N years, experienced developers will get excellent results through prompts because they have **20 years of experience**. That experience isn’t syntax knowledge, but understanding of:

* Architectural patterns
* Design principles
* Trade-offs
* Potential pitfalls

That’s why those who “get it” will get great results, and those who don’t will end up with a “brown substance.”

**AI assistants** aren’t a replacement for developers, but a tool that multiplies our speed. Experience and architectural understanding become even more important. And it will be harder for newcomers, because they’ll have to study technologies deliberately to get high-quality output from neural networks.

## Takeaways

1. **AI assistants** are not a replacement, but an **instrument**.
2. **Experience and understanding** of architecture are critically important.
3. **Development speed** for experienced specialists will increase many times over.
4. **Beginners** will have to put in more effort to master the profession.
