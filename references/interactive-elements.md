# Interactive Elements Reference — Paper to Lesson

Complete implementation patterns for every interactive element. Read this before building any interactivity.

---

## 1. Math → English Translation Blocks

**The most important teaching element.** Shows an equation (left) with plain-English meaning (right), line by line.

### Structure

```html
<div class="math-translation animate-in">
  <div class="translation-labels">
    <span class="translation-label">EQUATION</span>
    <span class="translation-label">PLAIN ENGLISH</span>
  </div>
  <div class="math-translation-grid">
    <div class="math-side">
      <div class="math-line" data-line="1">
        <span class="math-symbol" style="color: var(--color-component-1)">Q</span> = X · W<sub>Q</sub>
      </div>
      <div class="math-line" data-line="2">
        <span class="math-symbol" style="color: var(--color-component-2)">K</span> = X · W<sub>K</sub>
      </div>
      <!-- more lines -->
    </div>
    <div class="english-side">
      <div class="english-line" data-line="1">
        <span class="line-marker" style="background: var(--color-component-1)"></span>
        Create the "what am I looking for?" vector for each word
      </div>
      <div class="english-line" data-line="2">
        <span class="line-marker" style="background: var(--color-component-2)"></span>
        Create the "what do I contain?" vector for each word
      </div>
      <!-- more lines -->
    </div>
  </div>
</div>
```

### Styles

```css
.math-translation {
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}

.translation-labels {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0;
}

.translation-label {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  padding: var(--space-3) var(--space-5);
  font-weight: 500;
}

.translation-label:first-child {
  background: var(--color-bg-code);
  color: var(--color-text-inverse);
}

.translation-label:last-child {
  background: var(--color-surface-warm);
  color: var(--color-text-secondary);
  border-left: 3px solid var(--color-accent);
}

.math-translation-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  min-height: 0;
}

.math-side {
  background: var(--color-bg-code);
  color: var(--color-text-inverse);
  padding: var(--space-5);
  font-family: var(--font-mono);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  white-space: pre-wrap;        /* CRITICAL: never horizontal scroll */
  word-wrap: break-word;
}

.english-side {
  background: var(--color-surface-warm);
  padding: var(--space-5);
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  border-left: 3px solid var(--color-accent);
}

.math-line, .english-line {
  padding: var(--space-1) 0;
  min-height: 1.8em;
  display: flex;
  align-items: center;
}

.math-symbol {
  font-weight: 600;
}

.line-marker {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  display: inline-block;
  margin-right: var(--space-3);
  flex-shrink: 0;
}

/* Hover highlight: hovering a line highlights its pair */
.math-line:hover, .english-line:hover {
  background: rgba(255, 255, 255, 0.05);
}

/* Responsive: stack on mobile */
@media (max-width: 768px) {
  .math-translation-grid,
  .translation-labels {
    grid-template-columns: 1fr;
  }
  .english-side {
    border-left: none;
    border-top: 3px solid var(--color-accent);
  }
  .translation-label:last-child {
    border-left: none;
    border-top: 3px solid var(--color-accent);
  }
}
```

### Interactivity (optional: hover pairing)

```javascript
document.querySelectorAll('.math-translation-grid').forEach(grid => {
  const mathLines = grid.querySelectorAll('.math-line');
  const engLines = grid.querySelectorAll('.english-line');

  mathLines.forEach((line, i) => {
    line.addEventListener('mouseenter', () => {
      if (engLines[i]) engLines[i].style.background = 'rgba(var(--color-accent), 0.08)';
    });
    line.addEventListener('mouseleave', () => {
      if (engLines[i]) engLines[i].style.background = '';
    });
  });

  engLines.forEach((line, i) => {
    line.addEventListener('mouseenter', () => {
      if (mathLines[i]) mathLines[i].style.background = 'rgba(255,255,255,0.06)';
    });
    line.addEventListener('mouseleave', () => {
      if (mathLines[i]) mathLines[i].style.background = '';
    });
  });
});
```

---

## 2. Pipeline / Data Flow Animation

**Use case:** Visualize how data transforms as it moves through a model's pipeline (e.g., input text → tokenizer → encoder → attention → output).

### Structure

```html
<div class="pipeline-animation animate-in" id="pipeline-1">
  <div class="pipeline-controls">
    <button class="pipeline-btn" data-action="next">Next Step</button>
    <button class="pipeline-btn" data-action="restart">Restart</button>
    <span class="pipeline-progress">Step <span class="current-step">0</span> / <span class="total-steps">5</span></span>
  </div>
  <div class="pipeline-viewport">
    <div class="pipeline-stage" data-step="1" style="--component-color: var(--color-component-1)">
      <div class="stage-icon">📝</div>
      <div class="stage-label">Input Text</div>
    </div>
    <div class="pipeline-arrow">→</div>
    <div class="pipeline-stage" data-step="2" style="--component-color: var(--color-component-2)">
      <div class="stage-icon">🔤</div>
      <div class="stage-label">Tokenizer</div>
    </div>
    <div class="pipeline-arrow">→</div>
    <div class="pipeline-stage" data-step="3" style="--component-color: var(--color-component-3)">
      <div class="stage-icon">🧠</div>
      <div class="stage-label">Encoder</div>
    </div>
    <div class="pipeline-arrow">→</div>
    <div class="pipeline-stage" data-step="4" style="--component-color: var(--color-component-4)">
      <div class="stage-icon">🎯</div>
      <div class="stage-label">Attention</div>
    </div>
    <div class="pipeline-arrow">→</div>
    <div class="pipeline-stage" data-step="5" style="--component-color: var(--color-component-5)">
      <div class="stage-icon">📤</div>
      <div class="stage-label">Output</div>
    </div>
    <div class="pipeline-packet" id="packet-1"></div>
  </div>
  <div class="pipeline-description">
    <p class="step-description" data-step="0">Click "Next Step" to trace how data flows through the model.</p>
    <p class="step-description" data-step="1" hidden>"The cat sat on the mat" enters the model as raw text.</p>
    <p class="step-description" data-step="2" hidden>The tokenizer breaks the sentence into tokens: ["The", "cat", "sat", "on", "the", "mat"]</p>
    <p class="step-description" data-step="3" hidden>Each token is converted into a number vector (embedding) that captures its meaning.</p>
    <p class="step-description" data-step="4" hidden>Attention lets each word "look at" every other word to understand context.</p>
    <p class="step-description" data-step="5" hidden>The model produces its output — a prediction, translation, or summary.</p>
  </div>
</div>
```

### Styles

```css
.pipeline-animation {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}

.pipeline-controls {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  margin-bottom: var(--space-6);
}

.pipeline-btn {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 500;
  padding: var(--space-2) var(--space-4);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-full);
  background: var(--color-surface);
  color: var(--color-text);
  cursor: pointer;
  transition: all var(--duration-fast) var(--ease-out);
}

.pipeline-btn:hover {
  background: var(--color-accent);
  color: var(--color-text-inverse);
  border-color: var(--color-accent);
}

.pipeline-progress {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  margin-left: auto;
}

.pipeline-viewport {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-8) var(--space-4);
  padding-bottom: var(--space-3);    /* room for scrollbar */
  position: relative;
  overflow-x: auto;                  /* scroll instead of wrap — prevents broken arrows */
  -webkit-overflow-scrolling: touch;
}

.pipeline-stage {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-4);
  border-radius: var(--radius-md);
  border: 2px solid var(--color-border);
  background: var(--color-surface);
  transition: all var(--duration-normal) var(--ease-out);
  min-width: 90px;
  text-align: center;
}

.pipeline-stage.active {
  border-color: var(--component-color);
  box-shadow: 0 0 20px color-mix(in srgb, var(--component-color) 25%, transparent);
  transform: scale(1.08);
}

.pipeline-stage.completed {
  border-color: var(--component-color);
  opacity: 0.7;
}

.stage-icon {
  font-size: var(--text-2xl);
}

.stage-label {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: 500;
  color: var(--color-text-secondary);
}

.pipeline-arrow {
  font-size: var(--text-xl);
  color: var(--color-text-muted);
  transition: color var(--duration-normal) var(--ease-out);
}

.pipeline-arrow.active {
  color: var(--color-accent);
}

.pipeline-description {
  margin-top: var(--space-6);
  padding: var(--space-4);
  background: var(--color-surface-warm);
  border-radius: var(--radius-md);
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  min-height: 3em;
}

@media (max-width: 480px) {
  .pipeline-viewport { flex-direction: column; }
  .pipeline-arrow { transform: rotate(90deg); }
}
```

### JavaScript

```javascript
document.querySelectorAll('.pipeline-animation').forEach(pipeline => {
  let currentStep = 0;
  const stages = pipeline.querySelectorAll('.pipeline-stage');
  const arrows = pipeline.querySelectorAll('.pipeline-arrow');
  const descriptions = pipeline.querySelectorAll('.step-description');
  const totalSteps = stages.length;
  const currentStepEl = pipeline.querySelector('.current-step');
  const totalStepsEl = pipeline.querySelector('.total-steps');

  totalStepsEl.textContent = totalSteps;

  function updateStep(step) {
    currentStep = step;
    currentStepEl.textContent = step;

    stages.forEach((s, i) => {
      s.classList.remove('active', 'completed');
      if (i + 1 === step) s.classList.add('active');
      else if (i + 1 < step) s.classList.add('completed');
    });

    arrows.forEach((a, i) => {
      a.classList.toggle('active', i + 1 < step);
    });

    descriptions.forEach(d => d.hidden = true);
    const desc = pipeline.querySelector(`.step-description[data-step="${step}"]`);
    if (desc) desc.hidden = false;
  }

  pipeline.querySelector('[data-action="next"]').addEventListener('click', () => {
    if (currentStep < totalSteps) updateStep(currentStep + 1);
  });

  pipeline.querySelector('[data-action="restart"]').addEventListener('click', () => {
    updateStep(0);
  });
});
```

---

## 3. Multiple-Choice Quizzes

**Use case:** Test understanding after each module. Scenario-based questions, not recall.

### Structure

```html
<div class="quiz animate-in" id="quiz-1">
  <h3 class="quiz-title">Check Your Understanding</h3>

  <div class="quiz-question" data-correct="b">
    <p class="question-text">A translation model is producing nonsensical output for long sentences but works fine for short ones. Based on what you learned about attention, what's most likely going wrong?</p>
    <div class="quiz-options">
      <label class="quiz-option">
        <input type="radio" name="q1" value="a">
        <span class="option-marker">A</span>
        <span class="option-text">The model doesn't have enough parameters</span>
      </label>
      <label class="quiz-option">
        <input type="radio" name="q1" value="b">
        <span class="option-marker">B</span>
        <span class="option-text">The attention mechanism is struggling to focus on relevant words when there are too many to choose from</span>
      </label>
      <label class="quiz-option">
        <input type="radio" name="q1" value="c">
        <span class="option-marker">C</span>
        <span class="option-text">The tokenizer is breaking long sentences incorrectly</span>
      </label>
    </div>
    <button class="quiz-check-btn">Check Answer</button>
    <div class="quiz-feedback" hidden></div>
  </div>

  <!-- More questions... -->
</div>
```

### Styles

```css
.quiz {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-8);
  box-shadow: var(--shadow-md);
  margin: var(--space-10) 0;
}

.quiz-title {
  font-family: var(--font-display);
  font-size: var(--text-2xl);
  font-weight: 600;
  margin-bottom: var(--space-8);
  color: var(--color-text);
}

.quiz-question {
  margin-bottom: var(--space-8);
  padding-bottom: var(--space-8);
  border-bottom: 1px solid var(--color-border);
}

.quiz-question:last-of-type {
  border-bottom: none;
  margin-bottom: 0;
  padding-bottom: 0;
}

.question-text {
  font-family: var(--font-body);
  font-size: var(--text-lg);
  line-height: var(--leading-normal);
  margin-bottom: var(--space-5);
  color: var(--color-text);
}

.quiz-options {
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
  margin-bottom: var(--space-5);
}

.quiz-option {
  display: flex;
  align-items: flex-start;
  gap: var(--space-3);
  padding: var(--space-4);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-md);
  cursor: pointer;
  transition: all var(--duration-fast) var(--ease-out);
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
}

.quiz-option:hover {
  border-color: var(--color-accent);
  background: var(--color-accent-light);
}

.quiz-option input[type="radio"] {
  display: none;
}

.quiz-option.selected {
  border-color: var(--color-accent);
  background: var(--color-accent-light);
}

.quiz-option.correct {
  border-color: var(--color-success);
  background: var(--color-success-light);
}

.quiz-option.incorrect {
  border-color: var(--color-error);
  background: var(--color-error-light);
}

.option-marker {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: var(--color-bg-alt);
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  font-weight: 600;
  flex-shrink: 0;
  transition: all var(--duration-fast) var(--ease-out);
}

.quiz-option.selected .option-marker {
  background: var(--color-accent);
  color: var(--color-text-inverse);
}

.quiz-option.correct .option-marker {
  background: var(--color-success);
  color: var(--color-text-inverse);
}

.quiz-option.incorrect .option-marker {
  background: var(--color-error);
  color: var(--color-text-inverse);
}

.option-text {
  flex: 1;
  padding-top: 2px;
}

.quiz-check-btn {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 600;
  padding: var(--space-3) var(--space-6);
  border: none;
  border-radius: var(--radius-full);
  background: var(--color-accent);
  color: var(--color-text-inverse);
  cursor: pointer;
  transition: all var(--duration-fast) var(--ease-out);
}

.quiz-check-btn:hover {
  transform: translateY(-1px);
  box-shadow: var(--shadow-md);
}

.quiz-check-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  transform: none;
}

.quiz-feedback {
  margin-top: var(--space-4);
  padding: var(--space-4);
  border-radius: var(--radius-md);
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
}

.quiz-feedback.correct {
  background: var(--color-success-light);
  border-left: 4px solid var(--color-success);
}

.quiz-feedback.incorrect {
  background: var(--color-error-light);
  border-left: 4px solid var(--color-error);
}
```

### JavaScript

```javascript
document.querySelectorAll('.quiz-question').forEach(question => {
  const correctAnswer = question.dataset.correct;
  const options = question.querySelectorAll('.quiz-option');
  const checkBtn = question.querySelector('.quiz-check-btn');
  const feedback = question.querySelector('.quiz-feedback');
  let selectedValue = null;
  let answered = false;

  options.forEach(option => {
    option.addEventListener('click', () => {
      if (answered) return;
      options.forEach(o => o.classList.remove('selected'));
      option.classList.add('selected');
      selectedValue = option.querySelector('input').value;
    });
  });

  checkBtn.addEventListener('click', () => {
    if (!selectedValue || answered) return;
    answered = true;
    checkBtn.disabled = true;

    options.forEach(option => {
      const value = option.querySelector('input').value;
      option.style.pointerEvents = 'none';
      if (value === correctAnswer) {
        option.classList.add('correct');
      } else if (value === selectedValue) {
        option.classList.add('incorrect');
      }
    });

    feedback.hidden = false;
    if (selectedValue === correctAnswer) {
      feedback.className = 'quiz-feedback correct';
      feedback.textContent = question.dataset.correctExplanation || 'Exactly right!';
    } else {
      feedback.className = 'quiz-feedback incorrect';
      feedback.textContent = question.dataset.incorrectExplanation || 'Not quite — review the section above.';
    }
  });
});
```

**Tip:** Store per-question explanations in `data-correct-explanation` and `data-incorrect-explanation` attributes on the `.quiz-question` div.

---

## 4. Architecture Diagrams (Interactive)

**Use case:** Show model architecture with clickable/hoverable components that reveal descriptions.

### Structure

```html
<div class="architecture-diagram animate-in">
  <div class="arch-title">Model Architecture</div>
  <div class="arch-viewport">
    <div class="arch-zone" style="--zone-color: var(--color-component-1)">
      <div class="zone-label">Encoder</div>
      <div class="arch-component" data-desc="Converts each input token into a dense vector representation that captures its meaning."
           style="--comp-color: var(--color-component-1)">
        <span class="comp-icon">📐</span>
        <span class="comp-name">Embedding Layer</span>
      </div>
      <div class="arch-connector">↓</div>
      <div class="arch-component" data-desc="Lets each word look at all other words in the sentence to understand context and relationships."
           style="--comp-color: var(--color-component-2)">
        <span class="comp-icon">🎯</span>
        <span class="comp-name">Self-Attention</span>
      </div>
      <div class="arch-connector">↓</div>
      <div class="arch-component" data-desc="A small neural network applied to each position independently, adding non-linear processing power."
           style="--comp-color: var(--color-component-3)">
        <span class="comp-icon">⚙️</span>
        <span class="comp-name">Feed-Forward Network</span>
      </div>
    </div>
    <!-- More zones (Decoder, Output, etc.) -->
  </div>
  <div class="arch-description-panel">
    <p>Hover or tap a component to see what it does.</p>
  </div>
</div>
```

### Styles

```css
.architecture-diagram {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}

.arch-title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 600;
  margin-bottom: var(--space-6);
  text-align: center;
}

.arch-viewport {
  display: flex;
  gap: var(--space-6);
  justify-content: center;
  flex-wrap: wrap;
  padding: var(--space-4);
}

.arch-zone {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-5);
  border: 2px dashed color-mix(in srgb, var(--zone-color) 30%, transparent);
  border-radius: var(--radius-lg);
  background: color-mix(in srgb, var(--zone-color) 4%, transparent);
  min-width: 160px;
}

.zone-label {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--zone-color);
  margin-bottom: var(--space-2);
}

.arch-component {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  background: var(--color-surface);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-md);
  cursor: pointer;
  transition: all var(--duration-fast) var(--ease-out);
  width: 100%;
}

.arch-component:hover, .arch-component.active {
  border-color: var(--comp-color);
  box-shadow: 0 0 12px color-mix(in srgb, var(--comp-color) 20%, transparent);
  transform: scale(1.03);
}

.comp-icon {
  font-size: var(--text-lg);
}

.comp-name {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 500;
}

.arch-connector {
  color: var(--color-text-muted);
  font-size: var(--text-lg);
}

.arch-description-panel {
  margin-top: var(--space-5);
  padding: var(--space-4);
  background: var(--color-surface-warm);
  border-radius: var(--radius-md);
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  min-height: 3em;
  transition: all var(--duration-normal) var(--ease-out);
}
```

### JavaScript

```javascript
document.querySelectorAll('.architecture-diagram').forEach(diagram => {
  const panel = diagram.querySelector('.arch-description-panel');
  const defaultText = panel.innerHTML;

  diagram.querySelectorAll('.arch-component').forEach(comp => {
    comp.addEventListener('mouseenter', () => {
      diagram.querySelectorAll('.arch-component').forEach(c => c.classList.remove('active'));
      comp.classList.add('active');
      panel.textContent = comp.dataset.desc;
    });

    comp.addEventListener('mouseleave', () => {
      comp.classList.remove('active');
      panel.innerHTML = defaultText;
    });

    // Touch support
    comp.addEventListener('click', (e) => {
      e.stopPropagation();
      diagram.querySelectorAll('.arch-component').forEach(c => c.classList.remove('active'));
      comp.classList.add('active');
      panel.textContent = comp.dataset.desc;
    });
  });
});
```

---

## 5. Code Example Blocks

**Use case:** Demonstrate concepts with short, heavily commented Python/pseudocode snippets.

### Structure

```html
<div class="code-example animate-in">
  <div class="code-header">
    <span class="code-language">PYTHON</span>
    <span class="code-title">Computing Attention Scores</span>
  </div>
  <div class="code-body">
    <pre><code><span class="code-comment"># Step 1: Each word creates a "query" — what am I looking for?</span>
<span class="code-variable">queries</span> <span class="code-operator">=</span> <span class="code-function">transform</span>(words, <span class="code-variable">weight_q</span>)

<span class="code-comment"># Step 2: Each word also creates a "key" — what do I contain?</span>
<span class="code-variable">keys</span> <span class="code-operator">=</span> <span class="code-function">transform</span>(words, <span class="code-variable">weight_k</span>)

<span class="code-comment"># Step 3: Compare every query with every key</span>
<span class="code-comment"># High score = these words are relevant to each other</span>
<span class="code-variable">scores</span> <span class="code-operator">=</span> <span class="code-function">dot_product</span>(<span class="code-variable">queries</span>, <span class="code-variable">keys</span>)

<span class="code-comment"># Step 4: Convert scores to percentages (0-100%)</span>
<span class="code-variable">attention_weights</span> <span class="code-operator">=</span> <span class="code-function">softmax</span>(<span class="code-variable">scores</span>)

<span class="code-comment"># Result: each word now knows how much to "pay attention"</span>
<span class="code-comment"># to every other word in the sentence</span></code></pre>
  </div>
  <div class="code-output">
    <span class="output-label">WHAT THIS PRODUCES</span>
    <p>For the sentence "The cat sat on the mat", the word "sat" pays 45% attention to "cat" (who's sitting?), 30% to "mat" (where?), and divides the rest among other words.</p>
  </div>
</div>
```

### Styles

```css
.code-example {
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}

.code-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-3) var(--space-5);
  background: #16182A;
  border-bottom: 1px solid rgba(255,255,255,0.06);
}

.code-language {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--color-accent);
  font-weight: 500;
}

.code-title {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  color: rgba(255,255,255,0.5);
}

.code-body {
  background: var(--color-bg-code);
  padding: var(--space-5);
  overflow-x: auto;
}

.code-body pre {
  margin: 0;
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  line-height: var(--leading-loose);
  white-space: pre-wrap;
  word-wrap: break-word;
}

.code-output {
  background: var(--color-surface-warm);
  padding: var(--space-4) var(--space-5);
  border-top: 3px solid var(--color-accent);
}

.output-label {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--color-text-muted);
  display: block;
  margin-bottom: var(--space-2);
}

.code-output p {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  color: var(--color-text);
  margin: 0;
}
```

---

## 6. Comparison Cards

**Use case:** Before/after, old-vs-new method, or tradeoff comparisons side by side.

### Structure

```html
<div class="comparison animate-in">
  <div class="comparison-grid">
    <div class="comparison-card" style="--card-color: var(--color-error)">
      <div class="comparison-badge">BEFORE THIS PAPER</div>
      <h4 class="comparison-heading">RNNs process words one at a time</h4>
      <ul class="comparison-list">
        <li>Slow — can't parallelize</li>
        <li>Forgets early words in long sentences</li>
        <li>Each word only sees words before it</li>
      </ul>
    </div>
    <div class="comparison-card" style="--card-color: var(--color-success)">
      <div class="comparison-badge">THIS PAPER'S APPROACH</div>
      <h4 class="comparison-heading">Attention processes all words at once</h4>
      <ul class="comparison-list">
        <li>Fast — fully parallelizable</li>
        <li>Every word connects to every other word</li>
        <li>No distance limit on relationships</li>
      </ul>
    </div>
  </div>
</div>
```

### Styles

```css
.comparison {
  margin: var(--space-8) 0;
}

.comparison-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--space-5);
}

.comparison-card {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-sm);
  border-top: 4px solid var(--card-color);
}

.comparison-badge {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--card-color);
  margin-bottom: var(--space-3);
}

.comparison-heading {
  font-family: var(--font-display);
  font-size: var(--text-lg);
  font-weight: 600;
  margin-bottom: var(--space-4);
  color: var(--color-text);
}

.comparison-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
}

.comparison-list li {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  padding-left: var(--space-5);
  position: relative;
  color: var(--color-text-secondary);
}

.comparison-list li::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0.6em;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--card-color);
}

@media (max-width: 768px) {
  .comparison-grid { grid-template-columns: 1fr; }
}
```

---

## 7. Concept Cards Grid

**Use case:** Replace bullet-point lists with visual cards. Show components, features, or key concepts.

### Structure

```html
<div class="concept-grid stagger-children animate-in">
  <div class="concept-card animate-in" style="--stagger-index: 0; --card-color: var(--color-component-1)">
    <div class="concept-icon" style="background: var(--card-color)">🔤</div>
    <h4 class="concept-title">Tokenization</h4>
    <p class="concept-desc">Breaking text into small pieces the model can understand</p>
  </div>
  <div class="concept-card animate-in" style="--stagger-index: 1; --card-color: var(--color-component-2)">
    <div class="concept-icon" style="background: var(--card-color)">📐</div>
    <h4 class="concept-title">Embeddings</h4>
    <p class="concept-desc">Converting tokens into number vectors that capture meaning</p>
  </div>
  <div class="concept-card animate-in" style="--stagger-index: 2; --card-color: var(--color-component-3)">
    <div class="concept-icon" style="background: var(--card-color)">🎯</div>
    <h4 class="concept-title">Attention</h4>
    <p class="concept-desc">Letting each word decide which other words are important</p>
  </div>
</div>
```

### Styles

```css
.concept-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: var(--space-5);
  margin: var(--space-8) 0;
}

.concept-card {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-sm);
  transition: all var(--duration-normal) var(--ease-out);
  border: 1px solid var(--color-border);
}

.concept-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
  border-color: var(--card-color);
}

.concept-icon {
  width: 44px;
  height: 44px;
  border-radius: var(--radius-md);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: var(--text-xl);
  margin-bottom: var(--space-4);
  color: white;
}

.concept-title {
  font-family: var(--font-display);
  font-size: var(--text-lg);
  font-weight: 600;
  margin-bottom: var(--space-2);
  color: var(--color-text);
}

.concept-desc {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  line-height: var(--leading-normal);
  color: var(--color-text-secondary);
  margin: 0;
}
```

---

## 8. Callout Boxes

**Use case:** Highlight "aha!" insights, broader AI context, or important caveats. Max 2 per module.

### Structure

```html
<div class="callout callout-insight animate-in">
  <div class="callout-icon">💡</div>
  <div class="callout-content">
    <div class="callout-title">Why This Matters Beyond This Paper</div>
    <p>The attention mechanism didn't just improve translation — it became the foundation of GPT, BERT, and every modern large language model. When people talk about "transformers" in AI, this is the core idea.</p>
  </div>
</div>
```

### Variants

```html
<!-- Insight / "Aha!" moment (accent color) -->
<div class="callout callout-insight">...</div>

<!-- Context / "Good to know" (info color) -->
<div class="callout callout-context">...</div>

<!-- Caution / Limitation (warning color) -->
<div class="callout callout-caution">...</div>
```

### Styles

```css
.callout {
  display: flex;
  gap: var(--space-4);
  padding: var(--space-5);
  border-radius: var(--radius-md);
  margin: var(--space-6) 0;
}

.callout-icon {
  font-size: var(--text-xl);
  flex-shrink: 0;
  margin-top: 2px;
}

.callout-title {
  font-family: var(--font-display);
  font-size: var(--text-base);
  font-weight: 600;
  margin-bottom: var(--space-2);
}

.callout-content p {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  margin: 0;
}

.callout-insight {
  background: var(--color-accent-light);
  border-left: 4px solid var(--color-accent);
}
.callout-insight .callout-title { color: var(--color-accent); }

.callout-context {
  background: var(--color-info-light);
  border-left: 4px solid var(--color-info);
}
.callout-context .callout-title { color: var(--color-info); }

.callout-caution {
  background: var(--color-warning-light);
  border-left: 4px solid var(--color-warning);
}
.callout-caution .callout-title { color: var(--color-warning); }
```

---

## 9. Flow Diagrams (Step-by-Step)

**Use case:** Show a sequence of processing steps as connected cards instead of numbered text.

### Structure

```html
<div class="flow-diagram animate-in">
  <div class="flow-step">
    <div class="flow-number">1</div>
    <div class="flow-content">
      <h4>Raw Text Input</h4>
      <p>"The cat sat on the mat"</p>
    </div>
  </div>
  <div class="flow-arrow">→</div>
  <div class="flow-step">
    <div class="flow-number">2</div>
    <div class="flow-content">
      <h4>Tokenize</h4>
      <p>["The", "cat", "sat", "on", "the", "mat"]</p>
    </div>
  </div>
  <div class="flow-arrow">→</div>
  <div class="flow-step">
    <div class="flow-number">3</div>
    <div class="flow-content">
      <h4>Embed</h4>
      <p>[0.23, -0.81, ...] for each token</p>
    </div>
  </div>
</div>
```

### Styles

```css
.flow-diagram {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  margin: var(--space-8) 0;
  overflow-x: auto;                  /* scroll instead of wrap — prevents broken arrows */
  -webkit-overflow-scrolling: touch;
  padding-bottom: var(--space-2);    /* room for scrollbar */
}

.flow-step {
  display: flex;
  align-items: flex-start;
  gap: var(--space-3);
  background: var(--color-surface);
  padding: var(--space-4);
  border-radius: var(--radius-md);
  border-left: 4px solid var(--color-accent);
  box-shadow: var(--shadow-sm);
  min-width: 150px;
  max-width: 220px;
}

.flow-number {
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: var(--color-accent);
  color: var(--color-text-inverse);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  font-weight: 600;
  flex-shrink: 0;
}

.flow-content h4 {
  font-family: var(--font-display);
  font-size: var(--text-sm);
  font-weight: 600;
  margin-bottom: var(--space-1);
}

.flow-content p {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  margin: 0;
  word-break: break-word;
}

.flow-arrow {
  font-size: var(--text-xl);
  color: var(--color-accent);
  font-weight: 700;
  flex-shrink: 0;
}

@media (max-width: 480px) {
  .flow-diagram { flex-direction: column; }
  .flow-arrow { transform: rotate(90deg); }
  .flow-step { max-width: 100%; width: 100%; }
}
```

---

## 10. Glossary Tooltips (CRITICAL)

**Every technical term** gets a tooltip. This is non-negotiable.

### HTML

Wrap any technical term in:
```html
<span class="term" data-definition="Plain-English definition here.">technical term</span>
```

### Styles

```css
.term {
  text-decoration: underline;
  text-decoration-style: dashed;
  text-decoration-color: var(--color-accent-muted);
  text-underline-offset: 3px;
  cursor: pointer;
  position: relative;
}

.term:hover {
  text-decoration-color: var(--color-accent);
  color: var(--color-accent);
}

/* Tooltip (appended to document.body, position: fixed) */
.glossary-tooltip {
  position: fixed;
  z-index: 10000;
  width: 300px;
  padding: var(--space-4);
  background: var(--color-bg-code);
  color: var(--color-text-inverse);
  border-radius: var(--radius-md);
  font-family: var(--font-body);
  font-size: var(--text-sm);
  line-height: var(--leading-normal);
  box-shadow: var(--shadow-xl);
  pointer-events: none;
  opacity: 0;
  transition: opacity var(--duration-fast) var(--ease-out);
}

.glossary-tooltip.visible {
  opacity: 1;
}

/* Arrow */
.glossary-tooltip::after {
  content: '';
  position: absolute;
  bottom: -6px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 6px solid transparent;
  border-right: 6px solid transparent;
  border-top: 6px solid var(--color-bg-code);
}

.glossary-tooltip.flip::after {
  bottom: auto;
  top: -6px;
  border-top: none;
  border-bottom: 6px solid var(--color-bg-code);
}
```

### JavaScript (CRITICAL — position: fixed + body append)

```javascript
(function() {
  let activeTooltip = null;

  function showTooltip(term) {
    hideTooltip();

    const tip = document.createElement('div');
    tip.className = 'glossary-tooltip';
    tip.textContent = term.dataset.definition;
    document.body.appendChild(tip);

    const rect = term.getBoundingClientRect();
    const tipWidth = 300;
    let left = rect.left + rect.width / 2 - tipWidth / 2;
    left = Math.max(8, Math.min(left, window.innerWidth - tipWidth - 8));

    const tipHeight = tip.offsetHeight;

    if (rect.top - tipHeight - 8 < 0) {
      // Not enough room above — flip below
      tip.style.top = (rect.bottom + 8) + 'px';
      tip.classList.add('flip');
    } else {
      tip.style.top = (rect.top - tipHeight - 8) + 'px';
    }

    tip.style.left = left + 'px';
    requestAnimationFrame(() => tip.classList.add('visible'));
    activeTooltip = tip;
  }

  function hideTooltip() {
    if (activeTooltip) {
      activeTooltip.remove();
      activeTooltip = null;
    }
  }

  // Desktop: hover
  document.querySelectorAll('.term').forEach(term => {
    term.addEventListener('mouseenter', () => showTooltip(term));
    term.addEventListener('mouseleave', hideTooltip);
  });

  // Mobile: tap to toggle
  document.addEventListener('click', (e) => {
    const term = e.target.closest('.term');
    if (term) {
      e.preventDefault();
      if (activeTooltip) {
        hideTooltip();
        showTooltip(term);
      } else {
        showTooltip(term);
      }
    } else {
      hideTooltip();
    }
  });
})();
```

**WHY position: fixed + body append?** Math↔English translation blocks and code blocks use `overflow: hidden`. Any tooltip with `position: absolute` inside these containers gets clipped and becomes invisible. By appending to `document.body` with `position: fixed`, tooltips always render on top of everything.

---

## 11. Numbered Step Cards (Vertical)

**Use case:** Step-by-step explanations as visual cards rather than numbered paragraphs.

### Structure

```html
<div class="step-cards stagger-children">
  <div class="step-card animate-in" style="--stagger-index: 0">
    <div class="step-number">1</div>
    <div class="step-body">
      <h4>Tokenize the input</h4>
      <p>The raw text is split into smaller units called tokens — roughly one per word or word-piece.</p>
    </div>
  </div>
  <div class="step-card animate-in" style="--stagger-index: 1">
    <div class="step-number">2</div>
    <div class="step-body">
      <h4>Create embeddings</h4>
      <p>Each token is converted to a vector — a list of numbers that represents its meaning in a way the model can process.</p>
    </div>
  </div>
  <!-- more steps -->
</div>
```

### Styles

```css
.step-cards {
  display: flex;
  flex-direction: column;
  gap: var(--space-4);
  margin: var(--space-8) 0;
}

.step-card {
  display: flex;
  gap: var(--space-4);
  background: var(--color-surface);
  padding: var(--space-5);
  border-radius: var(--radius-md);
  border-left: 4px solid var(--color-accent);
  box-shadow: var(--shadow-sm);
}

.step-number {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: var(--color-accent);
  color: var(--color-text-inverse);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  font-weight: 700;
  flex-shrink: 0;
}

.step-body h4 {
  font-family: var(--font-display);
  font-size: var(--text-base);
  font-weight: 600;
  margin-bottom: var(--space-1);
}

.step-body p {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  line-height: var(--leading-normal);
  color: var(--color-text-secondary);
  margin: 0;
}
```

---

## 12. Drag-and-Drop Matching

**Use case:** Match concepts to their definitions or components to their roles.

### Structure

```html
<div class="matching-exercise animate-in" id="match-1">
  <h3 class="matching-title">Match each component to its role</h3>
  <div class="matching-chips" id="chips-1">
    <div class="matching-chip" draggable="true" data-match="encoder">Encoder</div>
    <div class="matching-chip" draggable="true" data-match="decoder">Decoder</div>
    <div class="matching-chip" draggable="true" data-match="attention">Attention Head</div>
  </div>
  <div class="matching-zones">
    <div class="matching-zone" data-accepts="encoder">
      <div class="zone-description">Reads the entire input and creates a summary representation</div>
      <div class="zone-droparea">Drop here</div>
    </div>
    <div class="matching-zone" data-accepts="decoder">
      <div class="zone-description">Generates output one piece at a time, guided by the summary</div>
      <div class="zone-droparea">Drop here</div>
    </div>
    <div class="matching-zone" data-accepts="attention">
      <div class="zone-description">Decides which parts of the input are most relevant at each step</div>
      <div class="zone-droparea">Drop here</div>
    </div>
  </div>
  <div class="matching-feedback" hidden></div>
</div>
```

### Styles

```css
.matching-exercise {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}

.matching-title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 600;
  margin-bottom: var(--space-6);
}

.matching-chips {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-3);
  margin-bottom: var(--space-6);
}

.matching-chip {
  padding: var(--space-2) var(--space-4);
  background: var(--color-accent);
  color: var(--color-text-inverse);
  border-radius: var(--radius-full);
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  font-weight: 500;
  cursor: grab;
  transition: all var(--duration-fast) var(--ease-out);
  user-select: none;
}

.matching-chip:active { cursor: grabbing; }
.matching-chip.placed { opacity: 0.4; pointer-events: none; }

.matching-zones {
  display: flex;
  flex-direction: column;
  gap: var(--space-4);
}

.matching-zone {
  display: flex;
  align-items: center;
  gap: var(--space-4);
  padding: var(--space-4);
  border: 2px dashed var(--color-border);
  border-radius: var(--radius-md);
  transition: all var(--duration-fast) var(--ease-out);
}

.matching-zone.drag-over {
  border-color: var(--color-accent);
  background: var(--color-accent-light);
}

.matching-zone.correct {
  border-color: var(--color-success);
  border-style: solid;
  background: var(--color-success-light);
}

.matching-zone.incorrect {
  border-color: var(--color-error);
  border-style: solid;
  background: var(--color-error-light);
}

.zone-description {
  flex: 1;
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
}

.zone-droparea {
  min-width: 120px;
  padding: var(--space-3) var(--space-4);
  border: 1px dashed var(--color-text-muted);
  border-radius: var(--radius-full);
  text-align: center;
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
}

.matching-feedback {
  margin-top: var(--space-5);
  padding: var(--space-4);
  border-radius: var(--radius-md);
  font-family: var(--font-body);
  font-size: var(--text-base);
}
```

### JavaScript

```javascript
document.querySelectorAll('.matching-exercise').forEach(exercise => {
  const chips = exercise.querySelectorAll('.matching-chip');
  const zones = exercise.querySelectorAll('.matching-zone');

  // Desktop drag-and-drop
  chips.forEach(chip => {
    chip.addEventListener('dragstart', (e) => {
      e.dataTransfer.setData('text/plain', chip.dataset.match);
      chip.style.opacity = '0.5';
    });
    chip.addEventListener('dragend', () => {
      chip.style.opacity = '';
    });
  });

  zones.forEach(zone => {
    const droparea = zone.querySelector('.zone-droparea');

    zone.addEventListener('dragover', (e) => {
      e.preventDefault();
      zone.classList.add('drag-over');
    });

    zone.addEventListener('dragleave', () => {
      zone.classList.remove('drag-over');
    });

    zone.addEventListener('drop', (e) => {
      e.preventDefault();
      zone.classList.remove('drag-over');
      const matchId = e.dataTransfer.getData('text/plain');
      const chip = exercise.querySelector(`.matching-chip[data-match="${matchId}"]`);

      if (zone.dataset.accepts === matchId) {
        zone.classList.add('correct');
        droparea.textContent = chip.textContent;
        droparea.style.borderStyle = 'solid';
        droparea.style.background = 'var(--color-success)';
        droparea.style.color = 'white';
        chip.classList.add('placed');
      } else {
        zone.classList.add('incorrect');
        setTimeout(() => zone.classList.remove('incorrect'), 800);
      }
    });
  });

  // Touch support
  let draggedChip = null;
  let ghost = null;

  chips.forEach(chip => {
    chip.addEventListener('touchstart', (e) => {
      draggedChip = chip;
      ghost = chip.cloneNode(true);
      ghost.style.position = 'fixed';
      ghost.style.pointerEvents = 'none';
      ghost.style.opacity = '0.8';
      ghost.style.zIndex = '9999';
      document.body.appendChild(ghost);
      const touch = e.touches[0];
      ghost.style.left = (touch.clientX - 40) + 'px';
      ghost.style.top = (touch.clientY - 15) + 'px';
    }, { passive: true });
  });

  document.addEventListener('touchmove', (e) => {
    if (!ghost) return;
    const touch = e.touches[0];
    ghost.style.left = (touch.clientX - 40) + 'px';
    ghost.style.top = (touch.clientY - 15) + 'px';
  }, { passive: true });

  document.addEventListener('touchend', (e) => {
    if (!ghost || !draggedChip) return;
    ghost.remove();
    ghost = null;

    const touch = e.changedTouches[0];
    const target = document.elementFromPoint(touch.clientX, touch.clientY);
    const zone = target?.closest('.matching-zone');

    if (zone && zone.dataset.accepts === draggedChip.dataset.match) {
      zone.classList.add('correct');
      const droparea = zone.querySelector('.zone-droparea');
      droparea.textContent = draggedChip.textContent;
      droparea.style.borderStyle = 'solid';
      droparea.style.background = 'var(--color-success)';
      droparea.style.color = 'white';
      draggedChip.classList.add('placed');
    } else if (zone) {
      zone.classList.add('incorrect');
      setTimeout(() => zone.classList.remove('incorrect'), 800);
    }

    draggedChip = null;
  });
});
```

---

## 13. "Spot the Flaw" Challenge

**Use case:** Present a flawed explanation or incorrect application of the paper's method, and ask the reader to identify what's wrong.

### Structure

```html
<div class="spot-flaw animate-in">
  <h3 class="flaw-title">Spot the Flaw</h3>
  <p class="flaw-prompt">Someone made this claim about the paper's method. Can you find what's wrong?</p>
  <div class="flaw-statement">
    <div class="flaw-line" data-correct="false" data-hint="This part is fine.">
      "The transformer uses attention to process input sequences."
    </div>
    <div class="flaw-line" data-correct="false" data-hint="Correct — it does process all positions.">
      "It processes all positions in parallel rather than sequentially."
    </div>
    <div class="flaw-line" data-correct="true" data-explanation="Actually, standard transformers have no built-in sense of word order — that's why they need positional encodings added separately!">
      "The model inherently understands word order because of how attention works."
    </div>
  </div>
  <p class="flaw-instruction">Click the line that contains the error.</p>
</div>
```

### Styles

```css
.spot-flaw {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}

.flaw-title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 600;
  margin-bottom: var(--space-3);
}

.flaw-prompt {
  font-family: var(--font-body);
  font-size: var(--text-base);
  color: var(--color-text-secondary);
  margin-bottom: var(--space-5);
}

.flaw-statement {
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
  margin-bottom: var(--space-4);
}

.flaw-line {
  padding: var(--space-4);
  background: var(--color-surface-warm);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-md);
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  cursor: pointer;
  transition: all var(--duration-fast) var(--ease-out);
}

.flaw-line:hover {
  border-color: var(--color-accent);
}

.flaw-line.correct-pick {
  border-color: var(--color-success);
  background: var(--color-success-light);
}

.flaw-line.wrong-pick {
  border-color: var(--color-error);
  background: var(--color-error-light);
}

.flaw-explanation {
  margin-top: var(--space-4);
  padding: var(--space-4);
  border-radius: var(--radius-md);
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
}

.flaw-explanation.success {
  background: var(--color-success-light);
  border-left: 4px solid var(--color-success);
}

.flaw-explanation.hint {
  background: var(--color-warning-light);
  border-left: 4px solid var(--color-warning);
}

.flaw-instruction {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  color: var(--color-text-muted);
  font-style: italic;
}
```

### JavaScript

```javascript
document.querySelectorAll('.spot-flaw').forEach(challenge => {
  let solved = false;

  challenge.querySelectorAll('.flaw-line').forEach(line => {
    line.addEventListener('click', () => {
      if (solved) return;

      // Remove previous attempts
      challenge.querySelectorAll('.flaw-line').forEach(l => l.classList.remove('wrong-pick'));
      const oldFeedback = challenge.querySelector('.flaw-explanation');
      if (oldFeedback) oldFeedback.remove();

      const feedback = document.createElement('div');
      feedback.className = 'flaw-explanation';

      if (line.dataset.correct === 'true') {
        solved = true;
        line.classList.add('correct-pick');
        feedback.classList.add('success');
        feedback.textContent = line.dataset.explanation;
        challenge.querySelectorAll('.flaw-line').forEach(l => l.style.pointerEvents = 'none');
      } else {
        line.classList.add('wrong-pick');
        feedback.classList.add('hint');
        feedback.textContent = line.dataset.hint || 'Not this one — try another line.';
        setTimeout(() => line.classList.remove('wrong-pick'), 1200);
      }

      challenge.appendChild(feedback);
    });
  });
});
```

---

## 14. Results / Benchmark Visualization

**Use case:** Make paper results tangible instead of just listing numbers.

### Structure

```html
<div class="results-viz animate-in">
  <h3 class="results-title">How It Performed</h3>
  <div class="results-bars">
    <div class="result-row">
      <span class="result-label">This Paper's Method</span>
      <div class="result-bar-track">
        <div class="result-bar highlight" style="width: 85%">
          <span class="result-value">41.8 BLEU</span>
        </div>
      </div>
    </div>
    <div class="result-row">
      <span class="result-label">Previous Best (RNN)</span>
      <div class="result-bar-track">
        <div class="result-bar" style="width: 72%">
          <span class="result-value">35.7 BLEU</span>
        </div>
      </div>
    </div>
    <div class="result-row">
      <span class="result-label">Basic Baseline</span>
      <div class="result-bar-track">
        <div class="result-bar" style="width: 55%">
          <span class="result-value">27.3 BLEU</span>
        </div>
      </div>
    </div>
  </div>
  <p class="results-caption">Higher BLEU score = better translation quality. This paper improved over the previous best by 17%.</p>
</div>
```

### Styles

```css
.results-viz {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}

.results-title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 600;
  margin-bottom: var(--space-6);
}

.results-bars {
  display: flex;
  flex-direction: column;
  gap: var(--space-4);
}

.result-row {
  display: flex;
  align-items: center;
  gap: var(--space-4);
}

.result-label {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 500;
  min-width: 180px;
  color: var(--color-text-secondary);
}

.result-bar-track {
  flex: 1;
  height: 36px;
  background: var(--color-bg-alt);
  border-radius: var(--radius-full);
  overflow: hidden;
}

.result-bar {
  height: 100%;
  background: var(--color-text-muted);
  border-radius: var(--radius-full);
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding-right: var(--space-4);
  transition: width 1s var(--ease-out);
}

.result-bar.highlight {
  background: var(--color-accent);
}

.result-value {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: 600;
  color: white;
  white-space: nowrap;
}

.results-caption {
  margin-top: var(--space-4);
  font-family: var(--font-body);
  font-size: var(--text-sm);
  color: var(--color-text-muted);
  line-height: var(--leading-normal);
}

@media (max-width: 480px) {
  .result-row { flex-direction: column; align-items: stretch; gap: var(--space-1); }
  .result-label { min-width: auto; }
}
```

---

## 15. Navigation Bar with Progress

### Structure

```html
<nav class="lesson-nav" id="lesson-nav">
  <div class="nav-inner">
    <div class="nav-title">Paper Title — Simplified</div>
    <div class="nav-modules">
      <button class="nav-module-btn active" data-module="0">1</button>
      <button class="nav-module-btn" data-module="1">2</button>
      <button class="nav-module-btn" data-module="2">3</button>
      <!-- one per module -->
    </div>
    <div class="nav-progress-bar">
      <div class="nav-progress-fill" id="progress-fill"></div>
    </div>
  </div>
</nav>
```

### Styles

```css
.lesson-nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: var(--nav-height);
  background: rgba(248, 246, 241, 0.95);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-bottom: 1px solid var(--color-border);
  z-index: 1000;
}

.nav-inner {
  max-width: var(--content-width-wide);
  margin: 0 auto;
  height: 100%;
  display: flex;
  align-items: center;
  padding: 0 var(--space-5);
  gap: var(--space-4);
}

.nav-title {
  font-family: var(--font-display);
  font-size: var(--text-sm);
  font-weight: 600;
  color: var(--color-text);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 250px;
}

.nav-modules {
  display: flex;
  gap: var(--space-2);
  margin-left: auto;
}

.nav-module-btn {
  width: 28px;
  height: 28px;
  border-radius: 50%;
  border: 1px solid var(--color-border);
  background: var(--color-surface);
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: 500;
  cursor: pointer;
  transition: all var(--duration-fast) var(--ease-out);
  color: var(--color-text-secondary);
}

.nav-module-btn.active {
  background: var(--color-accent);
  color: var(--color-text-inverse);
  border-color: var(--color-accent);
}

.nav-module-btn.visited {
  background: color-mix(in srgb, var(--color-accent) 15%, white);
  border-color: var(--color-accent);
}

.nav-progress-bar {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: var(--color-border);
}

.nav-progress-fill {
  height: 100%;
  background: var(--color-accent);
  width: 0%;
  transition: width var(--duration-normal) var(--ease-out);
}

@media (max-width: 480px) {
  .nav-title { display: none; }
}
```

### JavaScript

```javascript
(function() {
  const modules = document.querySelectorAll('.module');
  const buttons = document.querySelectorAll('.nav-module-btn');
  const progressFill = document.getElementById('progress-fill');

  // Scroll to module on nav button click
  buttons.forEach(btn => {
    btn.addEventListener('click', () => {
      const index = parseInt(btn.dataset.module);
      modules[index]?.scrollIntoView({ behavior: 'smooth' });
    });
  });

  // Update active module on scroll
  let ticking = false;
  window.addEventListener('scroll', () => {
    if (!ticking) {
      requestAnimationFrame(() => {
        const scrollY = window.scrollY;
        const docHeight = document.documentElement.scrollHeight - window.innerHeight;
        const progress = (scrollY / docHeight) * 100;
        progressFill.style.width = progress + '%';

        let activeIndex = 0;
        modules.forEach((mod, i) => {
          const rect = mod.getBoundingClientRect();
          if (rect.top <= window.innerHeight / 3) activeIndex = i;
        });

        buttons.forEach((btn, i) => {
          btn.classList.remove('active');
          if (i < activeIndex) btn.classList.add('visited');
          if (i === activeIndex) btn.classList.add('active');
        });

        ticking = false;
      });
      ticking = true;
    }
  }, { passive: true });

  // Keyboard navigation
  document.addEventListener('keydown', (e) => {
    if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') return;
    const activeBtn = document.querySelector('.nav-module-btn.active');
    const currentIndex = activeBtn ? parseInt(activeBtn.dataset.module) : 0;

    if (e.key === 'ArrowDown' || e.key === 'j') {
      e.preventDefault();
      const next = Math.min(currentIndex + 1, modules.length - 1);
      modules[next]?.scrollIntoView({ behavior: 'smooth' });
    } else if (e.key === 'ArrowUp' || e.key === 'k') {
      e.preventDefault();
      const prev = Math.max(currentIndex - 1, 0);
      modules[prev]?.scrollIntoView({ behavior: 'smooth' });
    }
  });
})();
```

---

## 16. Research Context Timeline

**Use case:** Show where this paper fits in the research timeline — what came before, what this paper introduced, and what it enabled afterward. Helps readers understand the paper's significance in context.

### Structure

```html
<div class="research-timeline animate-in">
  <h3 class="timeline-title">Where This Paper Fits</h3>
  <div class="timeline-track">
    <div class="timeline-node" data-year="2014">
      <div class="timeline-year">2014</div>
      <div class="timeline-dot"></div>
      <div class="timeline-card">
        <h4>Seq2Seq</h4>
        <p>RNNs for translation — slow, forgets long sentences</p>
      </div>
    </div>
    <div class="timeline-connector"></div>
    <div class="timeline-node current" data-year="2017">
      <div class="timeline-year">2017</div>
      <div class="timeline-dot"></div>
      <div class="timeline-card">
        <h4>Attention Is All You Need</h4>
        <p>Replaced RNNs with pure attention — faster, better at long-range context</p>
      </div>
    </div>
    <div class="timeline-connector"></div>
    <div class="timeline-node" data-year="2018">
      <div class="timeline-year">2018</div>
      <div class="timeline-dot"></div>
      <div class="timeline-card">
        <h4>BERT</h4>
        <p>Used the encoder half for understanding text — enabled transfer learning</p>
      </div>
    </div>
  </div>
</div>
```

### Styles

```css
.research-timeline {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}

.timeline-title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 600;
  margin-bottom: var(--space-6);
}

.timeline-track {
  display: flex;
  align-items: flex-start;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  padding-bottom: var(--space-3);
  gap: 0;
}

.timeline-node {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 180px;
  flex-shrink: 0;
}

.timeline-year {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: 600;
  color: var(--color-text-muted);
  margin-bottom: var(--space-2);
}

.timeline-dot {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: var(--color-border);
  border: 3px solid var(--color-surface);
  box-shadow: 0 0 0 2px var(--color-border);
  z-index: 1;
}

.timeline-node.current .timeline-dot {
  background: var(--color-accent);
  box-shadow: 0 0 0 2px var(--color-accent), 0 0 12px color-mix(in srgb, var(--color-accent) 30%, transparent);
}

.timeline-node.current .timeline-year {
  color: var(--color-accent);
  font-weight: 700;
}

.timeline-connector {
  height: 2px;
  flex: 1;
  min-width: 40px;
  background: var(--color-border);
  margin-top: calc(var(--text-xs) * 1.15 + var(--space-2) + 7px);
}

.timeline-card {
  margin-top: var(--space-3);
  padding: var(--space-3);
  background: var(--color-surface-warm);
  border-radius: var(--radius-md);
  text-align: center;
  max-width: 180px;
}

.timeline-node.current .timeline-card {
  border: 2px solid var(--color-accent);
  background: var(--color-accent-light);
}

.timeline-card h4 {
  font-family: var(--font-display);
  font-size: var(--text-sm);
  font-weight: 600;
  margin-bottom: var(--space-1);
}

.timeline-card p {
  font-family: var(--font-body);
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  margin: 0;
  line-height: var(--leading-normal);
}

@media (max-width: 480px) {
  .timeline-track {
    flex-direction: column;
    align-items: stretch;
  }
  .timeline-node {
    flex-direction: row;
    gap: var(--space-3);
    min-width: auto;
  }
  .timeline-connector {
    width: 2px;
    height: 24px;
    min-width: 2px;
    margin-top: 0;
    margin-left: calc(var(--text-xs) * 3 + var(--space-3) + 7px);
  }
  .timeline-card {
    text-align: left;
    max-width: none;
    flex: 1;
  }
}
```

---

## 17. Key Contribution Cards

**Use case:** Highlight the 2-4 novel contributions of the paper in a visually prominent way. Typically used in the first or second module.

### Structure

```html
<div class="contributions animate-in stagger-children">
  <div class="contribution-card animate-in" style="--stagger-index: 0; --card-color: var(--color-component-1)">
    <div class="contribution-number">1</div>
    <div class="contribution-body">
      <h4>Self-Attention Replaces Recurrence</h4>
      <p>The model processes all words simultaneously instead of one at a time, making it dramatically faster to train.</p>
    </div>
  </div>
  <div class="contribution-card animate-in" style="--stagger-index: 1; --card-color: var(--color-component-2)">
    <div class="contribution-number">2</div>
    <div class="contribution-body">
      <h4>Multi-Head Attention</h4>
      <p>Instead of one attention pattern, the model learns 8 different ways to relate words — like reading a sentence for grammar, meaning, and tone all at once.</p>
    </div>
  </div>
  <div class="contribution-card animate-in" style="--stagger-index: 2; --card-color: var(--color-component-3)">
    <div class="contribution-number">3</div>
    <div class="contribution-body">
      <h4>Positional Encoding</h4>
      <p>Since there's no recurrence to track word order, the model adds a unique "position signal" to each word so it knows where everything sits in the sentence.</p>
    </div>
  </div>
</div>
```

### Styles

```css
.contributions {
  display: flex;
  flex-direction: column;
  gap: var(--space-4);
  margin: var(--space-8) 0;
}

.contribution-card {
  display: flex;
  align-items: flex-start;
  gap: var(--space-4);
  background: var(--color-surface);
  padding: var(--space-5);
  border-radius: var(--radius-md);
  border-left: 4px solid var(--card-color, var(--color-accent));
  box-shadow: var(--shadow-sm);
  transition: box-shadow var(--duration-fast) var(--ease-out);
}

.contribution-card:hover {
  box-shadow: var(--shadow-md);
}

.contribution-number {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: var(--card-color, var(--color-accent));
  color: var(--color-text-inverse);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-display);
  font-size: var(--text-lg);
  font-weight: 700;
  flex-shrink: 0;
}

.contribution-body h4 {
  font-family: var(--font-display);
  font-size: var(--text-lg);
  font-weight: 600;
  margin-bottom: var(--space-2);
}

.contribution-body p {
  font-family: var(--font-body);
  font-size: var(--text-base);
  color: var(--color-text-secondary);
  line-height: var(--leading-normal);
  margin: 0;
}
```

---

## 18. Ablation Study Visualizer

**Use case:** Show what happens when you remove parts of the model. Papers often include ablation studies — this makes them interactive and visual instead of a dry table.

### Structure

```html
<div class="ablation-viz animate-in">
  <h3 class="ablation-title">What Happens When You Remove Parts?</h3>
  <p class="ablation-subtitle">Toggle components off to see how performance changes.</p>
  <div class="ablation-rows">
    <div class="ablation-row baseline">
      <div class="ablation-label">Full Model</div>
      <div class="ablation-bar-track">
        <div class="ablation-bar" style="--bar-width: 100%">
          <span class="ablation-value">41.8 BLEU</span>
        </div>
      </div>
    </div>
    <div class="ablation-row" data-ablation="multi-head">
      <label class="ablation-toggle">
        <input type="checkbox" checked>
        <span class="toggle-slider"></span>
      </label>
      <div class="ablation-label">Multi-Head (→ Single Head)</div>
      <div class="ablation-bar-track">
        <div class="ablation-bar" data-full="100" data-reduced="87" style="--bar-width: 100%">
          <span class="ablation-value">41.8</span>
        </div>
      </div>
    </div>
    <div class="ablation-row" data-ablation="pos-encoding">
      <label class="ablation-toggle">
        <input type="checkbox" checked>
        <span class="toggle-slider"></span>
      </label>
      <div class="ablation-label">Positional Encoding</div>
      <div class="ablation-bar-track">
        <div class="ablation-bar" data-full="100" data-reduced="72" style="--bar-width: 100%">
          <span class="ablation-value">41.8</span>
        </div>
      </div>
    </div>
  </div>
  <div class="ablation-description">
    <p>Toggle switches to see the impact of removing each component. The full model achieves the best score.</p>
  </div>
</div>
```

### Styles

```css
.ablation-viz {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}

.ablation-title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 600;
  margin-bottom: var(--space-2);
}

.ablation-subtitle {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  color: var(--color-text-muted);
  margin-bottom: var(--space-6);
}

.ablation-rows {
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
}

.ablation-row {
  display: flex;
  align-items: center;
  gap: var(--space-3);
}

.ablation-row.baseline {
  padding-bottom: var(--space-3);
  border-bottom: 2px solid var(--color-border);
  margin-bottom: var(--space-2);
}

.ablation-label {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 500;
  min-width: 200px;
  color: var(--color-text-secondary);
}

.ablation-bar-track {
  flex: 1;
  height: 32px;
  background: var(--color-bg-alt);
  border-radius: var(--radius-full);
  overflow: hidden;
}

.ablation-bar {
  height: 100%;
  width: var(--bar-width);
  background: var(--color-accent);
  border-radius: var(--radius-full);
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding-right: var(--space-3);
  transition: width 0.6s var(--ease-out), background 0.3s;
}

.ablation-row.baseline .ablation-bar {
  background: var(--color-accent);
}

.ablation-bar.reduced {
  background: var(--color-warning);
}

.ablation-value {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: 600;
  color: white;
  white-space: nowrap;
}

.ablation-toggle {
  position: relative;
  width: 40px;
  height: 22px;
  flex-shrink: 0;
}

.ablation-toggle input {
  opacity: 0;
  width: 0;
  height: 0;
}

.toggle-slider {
  position: absolute;
  inset: 0;
  background: var(--color-border);
  border-radius: var(--radius-full);
  cursor: pointer;
  transition: background var(--duration-fast);
}

.toggle-slider::before {
  content: '';
  position: absolute;
  width: 16px;
  height: 16px;
  left: 3px;
  top: 3px;
  background: white;
  border-radius: 50%;
  transition: transform var(--duration-fast);
}

.ablation-toggle input:checked + .toggle-slider {
  background: var(--color-accent);
}

.ablation-toggle input:checked + .toggle-slider::before {
  transform: translateX(18px);
}

.ablation-description {
  margin-top: var(--space-4);
  padding: var(--space-3);
  background: var(--color-surface-warm);
  border-radius: var(--radius-md);
  font-family: var(--font-body);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  line-height: var(--leading-normal);
}

@media (max-width: 480px) {
  .ablation-row { flex-wrap: wrap; }
  .ablation-label { min-width: auto; flex: 1 1 100%; }
}
```

### JavaScript

```javascript
document.querySelectorAll('.ablation-viz').forEach(viz => {
  viz.querySelectorAll('.ablation-row[data-ablation]').forEach(row => {
    const checkbox = row.querySelector('input[type="checkbox"]');
    const bar = row.querySelector('.ablation-bar');
    const valueEl = row.querySelector('.ablation-value');
    const fullWidth = parseFloat(bar.dataset.full);
    const reducedWidth = parseFloat(bar.dataset.reduced);
    const baselineValue = parseFloat(viz.querySelector('.baseline .ablation-value').textContent);

    checkbox.addEventListener('change', () => {
      if (checkbox.checked) {
        bar.style.setProperty('--bar-width', fullWidth + '%');
        bar.classList.remove('reduced');
        valueEl.textContent = baselineValue.toFixed(1);
      } else {
        bar.style.setProperty('--bar-width', reducedWidth + '%');
        bar.classList.add('reduced');
        valueEl.textContent = (baselineValue * reducedWidth / 100).toFixed(1);
      }
    });
  });
});
```

---

## 19. Related Work Comparison Matrix

**Use case:** Compare this paper's approach against related work across multiple dimensions. Replaces the typical "Related Work" wall of text with a scannable visual.

### Structure

```html
<div class="comparison-matrix animate-in">
  <h3 class="matrix-title">How It Compares</h3>
  <div class="matrix-scroll">
    <table class="matrix-table">
      <thead>
        <tr>
          <th class="matrix-dimension-header">Dimension</th>
          <th>RNN Seq2Seq</th>
          <th>ConvSeq2Seq</th>
          <th class="matrix-highlight">Transformer (this paper)</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="matrix-dimension">Parallelizable?</td>
          <td><span class="matrix-no">No</span></td>
          <td><span class="matrix-partial">Partially</span></td>
          <td class="matrix-highlight"><span class="matrix-yes">Fully</span></td>
        </tr>
        <tr>
          <td class="matrix-dimension">Long-range context</td>
          <td><span class="matrix-no">Weak</span></td>
          <td><span class="matrix-partial">Medium</span></td>
          <td class="matrix-highlight"><span class="matrix-yes">Strong</span></td>
        </tr>
        <tr>
          <td class="matrix-dimension">Training speed</td>
          <td>Days</td>
          <td>Hours</td>
          <td class="matrix-highlight"><span class="matrix-yes">3.5 days on 8 GPUs</span></td>
        </tr>
        <tr>
          <td class="matrix-dimension">BLEU (EN→DE)</td>
          <td>35.7</td>
          <td>—</td>
          <td class="matrix-highlight"><strong>41.8</strong></td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
```

### Styles

```css
.comparison-matrix {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}

.matrix-title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 600;
  margin-bottom: var(--space-5);
}

.matrix-scroll {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}

.matrix-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
  font-family: var(--font-body);
  font-size: var(--text-sm);
}

.matrix-table th,
.matrix-table td {
  padding: var(--space-3) var(--space-4);
  text-align: left;
  border-bottom: 1px solid var(--color-border);
  white-space: nowrap;
}

.matrix-table th {
  font-family: var(--font-display);
  font-weight: 600;
  font-size: var(--text-xs);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--color-text-secondary);
  background: var(--color-bg-alt);
  position: sticky;
  top: 0;
}

.matrix-highlight {
  background: color-mix(in srgb, var(--color-accent) 6%, transparent);
}

th.matrix-highlight {
  color: var(--color-accent);
  background: color-mix(in srgb, var(--color-accent) 10%, var(--color-bg-alt));
}

.matrix-dimension {
  font-weight: 500;
  color: var(--color-text);
}

.matrix-dimension-header {
  min-width: 140px;
}

.matrix-yes {
  color: var(--color-success);
  font-weight: 600;
}

.matrix-no {
  color: var(--color-error);
}

.matrix-partial {
  color: var(--color-warning);
}

@media (max-width: 480px) {
  .matrix-table {
    font-size: var(--text-xs);
  }
  .matrix-table th,
  .matrix-table td {
    padding: var(--space-2) var(--space-3);
  }
}
```
