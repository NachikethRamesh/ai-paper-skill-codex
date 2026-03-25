---
name: ai-paper-skill
description: Turn AI research papers into interactive, non-technical single-page HTML lessons with analogy-first explanations, visual modules, code examples, math-to-English translations, and quizzes. Use when a user asks to simplify, teach, explain, or create an educational lesson from an AI/ML paper or PDF for non-technical audiences.
---

# Paper to Lesson â€” AI Research Paper Simplifier

Turn any AI research paper (PDF) into a beautiful, interactive single-page HTML lesson that makes the paper's ideas accessible to non-technical readers.

## What This Skill Does

You read an AI research paper and produce a self-contained HTML page that:

- Breaks dense academic content into digestible, visual sections
- Explains math, algorithms, and technical jargon in plain language with everyday analogies
- Provides runnable code examples that demonstrate key ideas
- Uses animated flowcharts, diagrams, and data-flow visualizations
- Includes quizzes that test understanding, not memorization
- Defines every technical term with inline glossary tooltips

## Who This Is For

**Curious non-technical readers** â€” people who:

- Follow AI news and want to understand the actual papers behind the headlines
- Are business professionals, writers, designers, or students outside of CS
- Have heard terms like "transformer", "attention", "diffusion" but don't really know what they mean
- Want to go deeper than blog summaries but can't parse academic notation
- Want to build intuition about how AI systems work without needing a PhD

**This is NOT for:**

- ML researchers who want implementation details
- Engineers looking for production-ready code
- People studying for academic exams on these topics

## The 4 Phases

### Phase 1: Paper Analysis

Read the entire paper thoroughly and deeply BEFORE building anything. Understand:

1. **The Problem** â€” What real-world problem is this paper trying to solve? Why does it matter?
2. **The Key Insight** â€” What is the one core idea that makes this paper's approach novel? Every paper has a "trick" â€” find it. If there is more than one key trick, pick top 3 based on novelty, value, and real-life application.
3. **The Method** â€” How does the approach work, step by step? Trace the pipeline across each step from input to output.
4. **The Architecture** â€” What are the major components? How do they connect? What flows between them?
5. **The Math** â€” Identify every equation. For each one, understand what it computes in plain terms. What goes in, what comes out, and why it's shaped that way.
6. **The Results** â€” What did they measure? What baselines did they beat? What do the numbers actually mean? What does this mean to a non-technical person? What does this mean to the industry as a whole?
7. **The Limitations** â€” What doesn't work? What are the authors honest about? What do they gloss over?
8. **The Vocabulary** â€” Collect every technical term, acronym, and piece of jargon. You'll need plain-English definitions for all of them.
9. **The Historical Context** â€” What prior work does this build on? What was the state of the art before this paper? What came after? You'll need this for the Research Context Timeline.

**Do NOT ask the user to explain the paper to you.** You are the expert here. Read the PDF, figure out what it says, and teach it back simply.

### Phase 2: Lesson Design

Structure the lesson as 5-10 modules following this arc:

1. **The Big Picture** â€” What problem exists, why it matters, and what this paper proposes (no jargon yet). End with: "By the end of this lesson, you'll understand exactly how [method] works."
2. **The Core Idea** â€” The paper's key insights, explained with an analogy. This is the "aha!" moment.
3. **How It Works** â€” Step-by-step walkthrough of the every single method used. Animated pipeline diagrams. Input â†’ processing â†’ output.
4. **Under the Hood** â€” The important components explained individually. Math translated to English. Code examples showing what each piece does. Break down equation into its variables and explain each variable and mathematical operation.
5. **Seeing It in Action** â€” Concrete examples of the method working. Before/after comparisons. What the model actually produces. Show how data flows through each layer (if any) or module (if any) or code component (if any).
6. **How They Proved It** â€” Results explained. What the benchmarks mean. Why the numbers matter (or don't).
7. **The Bigger Picture** â€” Where this fits in the field. What it enables. What's still unsolved. Put it in the perspective on a non-technical person.

Adjust this structure to fit the paper. Some papers need more modules for complex methods; others can combine sections. But every lesson needs at minimum:

- A clear problem statement before any technical content
- At least one analogy-first explanation
- At least one animated diagram or flowchart
- At least one code example
- At least one quiz

**Per-module requirements:**

- 3-6 screens (scrollable sub-sections within each module)
- At least 1 "Math â†’ English" translation OR code example
- At least 1 interactive element (quiz, animation, diagram, or flowchart)
- 1-2 insight callout boxes connecting to broader AI/ML concepts
- One unique analogy per module (NEVER reuse the same analogy across modules)

**Mandatory interactive elements** (ALL must appear somewhere in the lesson):

1. **Pipeline Animation** â€” Animated step-by-step data flow showing how input transforms into output
2. **Math â†’ English Translations** â€” Equation on left, plain-English meaning on right, line by line
3. **Code Playgrounds** â€” Short, commented code snippets demonstrating key concepts (Python/pseudocode)
4. **Concept Quizzes** â€” At least one per module, testing understanding not recall
5. **Glossary Tooltips** â€” Every technical term defined on hover/tap
6. **Architecture Diagrams** â€” Visual component maps showing how pieces connect
7. **Comparison Cards** â€” Before/after, old-vs-new, or tradeoff comparisons
8. **Research Context Timeline** â€” Where this paper fits historically (at least once, typically in module 1 or the final module)
9. **Key Contribution Cards** â€” The paper's 2-4 novel contributions highlighted visually (at least once, typically in module 1 or 2)
10. **Related Work Comparison Matrix** â€” Table comparing this paper vs. related approaches across dimensions (at least once, in the "Bigger Picture" module)
11. **Ablation Study Visualizer** â€” Interactive toggles showing what happens when model components are removed (include when the paper has ablation results)

**Do NOT present the curriculum for approval.** The user wants a lesson, not a planning document. Just build it. 

### Phase 3: Build the Lesson

Generate a single self-contained HTML file with embedded CSS and JavaScript. No external dependencies except Google Fonts.

**Build order:**

1. **Foundation** â€” HTML skeleton, full CSS design system (copy from `references/design-system.md`), navigation with progress bar, scroll-snap behavior, keyboard navigation (arrow keys, j/k), scroll-triggered reveal animations
2. **One module at a time** â€” Build each module's content, visuals, and interactivity. Verify each module works before starting the next. Do NOT write all modules in one pass.
3. **Polish** â€” Transitions, mobile responsiveness, visual consistency across modules

**Technical requirements:**

- Self-contained HTML (only external resource: Google Fonts CDN)
- CSS `scroll-snap-type: y proximity` (NEVER `mandatory` â€” it traps users in long sections)
- `min-height: 100dvh` with `100vh` fallback for module sections
- Only animate `transform` and `opacity` for GPU performance
- Wrap all JavaScript in an IIFE to avoid global scope pollution
- `passive: true` on scroll listeners, throttle scroll handlers with `requestAnimationFrame`
- Touch-friendly interactions, keyboard navigation, ARIA attributes for accessibility

Start the design with by highlighting paper name and the team. For each section of every module, explain technical jargon as and when they presented in non-technical layman terms.

### Phase 4: Review & Deliver

Open the HTML file in the browser. Walk through what the lesson covers. Ask for feedback â€” is the difficulty level right? Any sections that need more or less detail?

---

## Content Philosophy

### Analogy First, Then Reality

Every technical concept gets introduced with an everyday analogy BEFORE any technical detail.

**Pattern:**
1. Here's something you already understand (the analogy)
2. The paper's idea works the same way (the bridge)
3. Here's exactly how they do it (the technical reality)

**Example:**
> "Imagine you're at a cocktail party. You can focus on one conversation even though dozens are happening around you â€” your brain is 'attending' to the relevant voice and filtering out noise. That's exactly what an attention mechanism does with data..."

**CRITICAL â€” No recycled analogies:**
- Each concept gets its own unique, carefully chosen analogy
- The analogy should feel *inevitable* for that concept, not just convenient
- If you catch yourself reusing an analogy, stop and find a better one
- Common traps to avoid: don't use "factory assembly line" for everything, don't use "library" for every search/retrieval concept

### Show, Don't Tell â€” Aggressively Visual

**Text limits (hard rules):**
- Maximum **2-3 sentences per text block**. If a 4th sentence appears, convert to a visual element.
- No text block wider than content width AND taller than ~4 lines
- **Every screen must be at least 50% visual** (diagrams, code, cards, animations)

**Convert text to visuals:**
- 3+ items in a list â†’ icon cards in a grid
- A sequence of steps â†’ animated flow diagram or numbered step cards
- "Component A sends data to Component B" â†’ animated data flow
- "This equation computes X" â†’ Mathâ†”English translation block
- Comparing two approaches â†’ side-by-side comparison columns
- Model architecture â†’ interactive component diagram
- Paper's contribution list â†’ Key Contribution Cards
- Related work / prior approaches â†’ Related Work Comparison Matrix
- Ablation study results table â†’ Ablation Study Visualizer
- Historical context / "building on prior work" â†’ Research Context Timeline

**Visual breathing room:**
- Generous spacing between elements
- Alternate full-width visuals with narrow text blocks for rhythm
- Each module needs at least one "hero visual" â€” a diagram, animation, or interactive element that dominates the screen

### Math â†’ English Translations

**The most valuable teaching element for non-technical readers.** Left side: the actual equation from the paper. Right side: line-by-line plain English explanation.

**Critical rules:**
- Show the REAL equation from the paper â€” never simplify or omit parts
- Each English line maps to one symbol, term, or operation
- Use conversational language: "How much to focus on each word" not "Compute scaled dot-product attention weights"
- Highlight the *why* â€” what does this equation *accomplish*, not just what it *computes*
- Color-code corresponding parts between equation and explanation

**Alignment and compactness rules:**
- The number of `.math-line` elements MUST equal the number of `.english-line` elements. Count them.
- Each English line explains its corresponding math line specifically â€” not a general description of the equation, but "what does THIS part do and WHY."
- Keep equations compact. Each `.math-line` should contain one conceptual unit (a variable definition, an operation, or a sub-expression). Don't split `softmax(QÂ·K^T/âˆšd_k)` across 4 separate lines â€” group logically.
- Order matters: math line 1's explanation must be English line 1. If hovering math line 3 highlights English line 3, they must actually correspond.
- If an equation has 4 meaningful parts, use 4 lines â€” not 7. Excessive splitting creates visual gaps and breaks the 1:1 mapping.

**Example:**
```
Equation side:           English side:
softmax(QK^T / âˆšd_k)V
                         Q = What am I looking for? (the query)
                         K = What do I contain? (the keys)
                         QK^T = How well does each item match what I need?
                         âˆšd_k = Scale it down so the numbers don't explode
                         softmax = Convert to percentages (they sum to 100%)
                         Ã— V = Use those percentages to create a weighted mix
```

**Example:**
```
# For the word "it", attention computes scores with every word:
attention_scores = {
    "The":      0.02,   # barely relevant
    "cat":      0.71,   # very relevant! "it" = "cat"
    "sat":      0.05,   # somewhat relevant
    "on":       0.01,   # barely relevant
    "the":      0.01,   # barely relevant
    "mat":      0.12,   # possible but less likely
    "because":  0.03,   # barely relevant
    "was":      0.03,   # barely relevant
    "tired":    0.02,   # barely relevant
}

# Sum = 1.0 (100%) â€” it's a probability distribution
```

### Code Examples That Teach

Code snippets should demonstrate concepts, not implement production systems.

**Rules:**
- Python or pseudocode only (most accessible)
- Maximum 15-20 lines per snippet
- Every line gets a comment explaining what it does in plain English
- Use descriptive variable names (`attention_scores` not `attn`)
- Show inputs and outputs with concrete example values
- If the paper's actual code is available and short enough, use it with annotations
- Code is for building intuition, not for copy-pasting into a project

### One Concept Per Screen

No walls of text. Each screen teaches exactly one idea. If you need more space, add another screen â€” screens are free.

### Glossary Tooltips â€” No Term Left Behind

**Every single technical term** gets a tooltip with a 1-2 sentence plain-English definition.

Be EXTREMELY aggressive with tooltips. If there is even a 1% chance a non-technical reader doesn't know a term, tooltip it. This includes:

- ML terms: transformer, attention, embedding, gradient, loss function, backpropagation, epoch, batch
- Math terms: vector, matrix, dot product, softmax, probability distribution, normalization
- CS terms: algorithm, parameter, inference, training, model, architecture, pipeline, latency
- Paper jargon: ablation study, baseline, SOTA, benchmark, downstream task, fine-tuning, pre-training
- Acronyms: ALWAYS tooltip on first use (GPU, TPU, NLP, CV, LLM, BERT, GPT, etc.)

**Vocabulary IS learning.** A key goal: readers walk away able to use precise technical vocabulary when discussing AI.

**Implementation:** `cursor: pointer` (NOT `cursor: help`). Tooltips use `position: fixed` appended to `document.body` (never clipped by parent overflow). See `references/interactive-elements.md` for full implementation.

### Quizzes Test Understanding, Not Recall

**What to quiz (best to worst):**

1. **"What would happen if..." scenarios** â€” Change one thing about the method. What breaks? What improves? Gold standard.
2. **Analogy extension** â€” "Based on the cocktail party analogy, what would 'multi-head attention' be like?" Tests whether they internalized the concept.
3. **Architecture reasoning** â€” "Why does the model need component X? What would go wrong without it?" Tests structural understanding.
4. **Real-world application** â€” "A company wants to do Y. Based on this paper, what approach would you recommend?" Tests transfer.

**What NOT to quiz:**
- Definitions â€” "What does transformer stand for?" That's what tooltips are for.
- Specific numbers â€” "What was the BLEU score?" Nobody memorizes benchmark numbers.
- Equation recall â€” "Write the attention formula." This isn't an exam.
- Author names or dates â€” Completely irrelevant to understanding.

**Quiz tone:**
- Wrong answers get encouraging, educational explanations: "Not quite â€” here's the key insight you might have missed..."
- Correct answers get brief reinforcement: "Exactly! This works because..."
- Never punitive. No scores. No "3 out of 5." These are thinking exercises, not tests.
- Wrong answer explanations should teach something the lesson didn't cover.

**Frequency:** One quiz per module. 3-5 questions per quiz.

### Make It Stick

- Use "Insight" callout boxes for connections to the broader AI landscape
- Give model components personality â€” they're characters in a story ("The Encoder reads the sentence and creates a summary. The Decoder takes that summary and writes a translation, one word at a time.")
- Humor where natural, never forced
- Connect everything back to real-world impact â€” why should anyone care?

---

## Common Failure Modes â€” Avoid These

### 1. Tooltip Clipping
Mathâ†”English blocks and code blocks use `overflow: hidden`. Tooltips with `position: absolute` get clipped inside them. **Fix:** Tooltips must use `position: fixed` + append to `document.body` + calculate position via `getBoundingClientRect()`.

### 2. Not Enough Tooltips
The #1 most common failure. Non-technical readers don't know: gradient descent, latent space, embedding, epoch, inference, token, logit, perplexity, cross-entropy, convolution, pooling, residual connection, layer norm, dropout, fine-tuning, pre-training, zero-shot, few-shot, RLHF, and hundreds more. **When in doubt, tooltip it.**

### 3. Walls of Text
Academic papers are dense. The temptation is to write dense explanations. Resist. **Max 2-3 sentences, then a visual.** Every screen 50%+ visual. Convert paragraphs to diagrams, cards, flow animations.

### 4. Recycled Analogies
Using "factory assembly line" for every processing pipeline. Using "library" for every search mechanism. **Each concept gets a unique analogy that feels inevitable for that specific idea.**

### 5. Skipping the "Why It Matters"
Jumping straight into how the method works without explaining why anyone should care. **Always start with the problem. Make the reader feel the pain point before introducing the solution.**

### 6. Over-Simplifying Math
Either showing raw equations with no explanation, or skipping them entirely. **Show the real equation AND the plain-English translation side by side.** Readers should see both.

### 7. Quiz Memory Tests
Asking "What is the attention formula?" or "Name three components of the transformer." **Quiz scenarios and reasoning, not facts.**

### 8. Scroll-Snap Mandatory
`scroll-snap-type: y mandatory` traps users in long modules. **Always use `proximity`.**

### 9. Module Quality Degradation
Later modules getting thin and rushed because you wrote everything in one pass. **Build one module at a time. Verify each works before starting the next.**

### 10. Missing Interactive Elements
Modules with only text and static images. **Every module needs at least one interactive element: quiz, animation, diagram, or code example.**

### 11. Code Horizontal Scrollbars
Code blocks that scroll horizontally on smaller screens. **Always use `white-space: pre-wrap`.**

### 12. Not Using 100dvh
Using `100vh` on mobile causes the bottom to be cut off by browser chrome. **Always use `min-height: 100dvh` with `100vh` fallback.**

### 13. Jargon Creep
Starting simple but gradually letting jargon pile up without tooltips as you get deeper into the paper. **Tooltip every term on first use per module, even if you defined it in a previous module.** Readers may jump between sections.

### 14. Equation Line Mismatch
The Mathâ†’English block has equation parts that don't align with their English explanations. Line 3 of the equation should be explained by line 3 of the English side. **Count the lines on both sides â€” they must match.** Each English line should explain the specific math part next to it, not give a generic description of the overall equation.

### 15. Missing Research Context
Presenting the paper in isolation without explaining what came before or what this paper changed. **Always include a Research Context Timeline** showing the paper's place in the field â€” what prior work it built on and what it enabled.

### 16. Pipeline Overflow
Pipeline animations and flow diagrams with `flex-wrap: wrap` that cause arrows to point into empty space when stages wrap to a new line. **Use `overflow-x: auto` instead of `flex-wrap: wrap`** so pipelines scroll horizontally rather than breaking into disconnected rows.

---

## Reference Files

Before building, read these reference files for implementation details:

- `references/design-system.md` â€” Complete CSS design tokens (colors, typography, spacing, shadows, animations). Copy the entire `:root` block into your CSS.
- `references/interactive-elements.md` â€” Implementation patterns for every interactive element (pipeline animations, math translations, quizzes, tooltips, diagrams, code blocks, comparison cards, etc.)
