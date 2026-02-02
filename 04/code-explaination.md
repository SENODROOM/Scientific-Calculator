# Interactive Math Editor Applications - Code Explanation

## Overview

These are two related web-based mathematical expression editors that use **MathQuill** for input and **KaTeX** for rendering. They demonstrate modern approaches to creating mathematical notation interfaces in browsers.

### The Two Applications

1. **`mathinteractiveeditor.html`** - Basic math editor with symbol insertion
2. **`index.html`** - Enhanced version with calculator functionality

---

## Technology Stack

### Core Libraries

| Library | Purpose | Version |
|---------|---------|---------|
| **MathQuill** | Interactive math input field | Latest from CDN |
| **KaTeX** | Mathematical notation rendering | 0.16.9 |
| **jQuery** | Required dependency for MathQuill | 3.7.1 |
| **Math.js** | Expression evaluation (index.html only) | v12 |

### Why These Libraries?

**MathQuill:**
- Provides WYSIWYG mathematical input
- Handles cursor positioning in complex expressions
- Converts user input to LaTeX automatically
- Browser-native, no server required

**KaTeX:**
- Fast LaTeX rendering engine
- Lighter than MathJax
- Client-side rendering
- Supports extensive mathematical notation

**Math.js:**
- Evaluates mathematical expressions
- Handles complex numbers, units, matrices
- Safe alternative to `eval()`

---

## Application 1: Basic Math Editor (`mathinteractiveeditor.html`)

### HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <!-- Dependencies -->
    <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/mathquill/build/mathquill.css">
    <script src="https://cdn.jsdelivr.net/npm/mathquill/build/mathquill.min.js"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css">
    <script src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js"></script>
</head>
```

**Dependency Loading Order:**
1. jQuery (required by MathQuill)
2. MathQuill CSS and JS
3. KaTeX CSS and JS

This order is crucial - MathQuill won't work without jQuery loaded first.

### Visual Design

```css
body {
    background: #f5f6fa;
    display: flex;
    justify-content: center;
    padding: 40px;
}

.container {
    width: 700px;
    background: white;
    border-radius: 12px;
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
}
```

**Design Features:**
- Centered layout with flexbox
- Clean white container on soft gray background
- Professional box shadow for depth
- Fixed width (700px) for consistent experience

### Toolbar System

```html
<div class="toolbar">
    <button data-latex="\\frac{}{}">Fraction</button>
    <button data-latex="\\sqrt{}">√</button>
    <button data-latex="^{}">x²</button>
    <button data-latex="_{}">xᵢ</button>
    <button data-latex="\\sum_{}^{}">Σ</button>
    <button data-latex="\\left( \\right)">()</button>
    <button data-latex="\\pi">π</button>
    <button data-latex="\\theta">θ</button>
    <button data-latex="+">+</button>
    <button data-latex="-">−</button>
</div>
```

**Button Grid Layout:**
```css
.toolbar {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 10px;
}
```
- 5 columns of equal width
- 10px spacing between buttons
- Responsive to container width

**Data Attributes:**
- Each button stores LaTeX command in `data-latex` attribute
- Example: `\\frac{}{}` creates a fraction template
- The `{}` placeholders indicate cursor positions

### JavaScript Architecture

#### Module Pattern

```javascript
const MathEditor = (() => {
    // Private variables and functions
    const MQ = MathQuill.getInterface(2);
    const editorSpan = document.getElementById("math-editor");
    // ...
    
    // Public API
    return {
        insertLatex,
        mathField
    };
})();
```

**Why Use This Pattern?**
- Encapsulation: Private variables aren't accessible globally
- Namespace: Only one global variable (`MathEditor`)
- Organization: Clear separation of concerns

#### MathQuill Initialization

```javascript
const mathField = MQ.MathField(editorSpan, {
    spaceBehavesLikeTab: true,
    handlers: {
        edit: updateOutputs
    }
});
```

**Configuration Options:**

**`spaceBehavesLikeTab: true`**
- Pressing space moves cursor like Tab
- Useful for navigating between fraction numerator/denominator
- Example: In `\frac{1}{2}`, space moves from numerator to denominator

**`handlers.edit`**
- Callback triggered on every edit
- Updates LaTeX and rendered outputs in real-time
- Provides live preview functionality

#### Output Update System

```javascript
function updateOutputs() {
    const latex = mathField.latex();
    latexOutput.textContent = latex;
    
    try {
        katex.render(latex, renderOutput, {
            throwOnError: false
        });
    } catch (e) {
        renderOutput.textContent = "Rendering error";
    }
}
```

**Process Flow:**
1. Get LaTeX string from MathQuill field
2. Display raw LaTeX in text output
3. Render with KaTeX in visual output
4. Handle errors gracefully

**Error Handling:**
- `throwOnError: false`: KaTeX won't crash on invalid LaTeX
- Fallback: Display "Rendering error" message
- Try-catch: Extra safety layer

#### Toolbar Event Handling

```javascript
document.querySelectorAll(".toolbar button").forEach(btn => {
    btn.addEventListener("click", () => {
        const latex = btn.getAttribute("data-latex");
        MathEditor.insertLatex(latex);
    });
});
```

**Event Delegation Alternative:**
Could be optimized with a single event listener on `.toolbar`:
```javascript
document.querySelector(".toolbar").addEventListener("click", (e) => {
    if (e.target.matches("button[data-latex]")) {
        MathEditor.insertLatex(e.target.dataset.latex);
    }
});
```

### User Experience Flow

1. **User clicks "Fraction" button**
   ```javascript
   // Button has data-latex="\\frac{}{}"
   insertLatex("\\frac{}{}");
   ```

2. **MathQuill creates editable fraction**
   ```
   Visual: [  ] / [  ]
           ↑ cursor here
   ```

3. **User types "1"**
   ```
   Visual: [1] / [  ]
   LaTeX: \frac{1}{}
   ```

4. **User presses Space (acts like Tab)**
   ```
   Visual: [1] / [  ]
               ↑ cursor moves here
   ```

5. **User types "2"**
   ```
   Visual: [1] / [2]
   LaTeX: \frac{1}{2}
   Rendered: ½
   ```

---

## Application 2: Math Editor + Calculator (`index.html`)

### Key Differences from Basic Version

| Feature | Basic Version | Calculator Version |
|---------|---------------|-------------------|
| **Libraries** | MathQuill, KaTeX, jQuery | + Math.js |
| **Toolbar** | 10 buttons (5 columns) | 12 buttons (6 columns) |
| **Functionality** | Display only | Evaluation capability |
| **Calculate Button** | None | Green "=" button |

### Enhanced Toolbar

```html
<div class="toolbar">
    <button data-latex="\\frac{}{}">Fraction</button>
    <button data-latex="\\sqrt{}">√</button>
    <button data-latex="^{}">x²</button>
    <button data-latex="_{}">xᵢ</button>
    <button data-latex="\\sum_{n=1}^{ }">Σ</button>
    <button data-latex="\\left( \\right)">()</button>
    <button data-latex="+">+</button>
    <button data-latex="-">−</button>
    <button data-latex="\\cdot">*</button>
    <button data-latex="/">÷</button>
    <button data-latex="\\pi">π</button>
    <button id="calcBtn">=</button>  <!-- NEW -->
</div>
```

**New Elements:**
- `\\cdot` for multiplication symbol (·)
- `/` for division
- `#calcBtn` for evaluation
- Grid changed to 6 columns

### Calculate Button Styling

```css
#calcBtn {
    background: #4CAF50;
    color: white;
}

#calcBtn:hover {
    background: #43a047;
}
```

**Visual Hierarchy:**
- Green color signals "action" button
- Stands out from gray toolbar buttons
- Hover effect provides feedback

### Calculator Module

This is the most complex part of the application.

#### Complete Code

```javascript
document.getElementById("calcBtn").addEventListener("click", () => {
    let latex = MathEditor.mathField.latex();
    
    try {
        // Remove LaTeX commands (\frac, \sqrt, etc.)
        const cleaned = latex.replace(/\\[a-zA-Z]+/g, "");
        
        // If letters remain → user typed variables like x or y
        if (/[a-zA-Z]/.test(cleaned)) {
            alert("Expression contains variables. Cannot evaluate.");
            return;
        }
        
        // Convert LaTeX → math.js expression
        const expr = latex
            .replace(/\\frac{([^}]*)}{([^}]*)}/g, "($1)/($2)")
            .replace(/\\sqrt{([^}]*)}/g, "sqrt($1)")
            .replace(/\\cdot/g, "*")
            .replace(/\\pi/g, "pi")
            .replace(/\\left|\\right/g, "");
        
        const result = math.evaluate(expr);
        
        MathEditor.mathField.latex(result.toString());
        
    } catch (err) {
        alert("Invalid numeric expression");
        console.error(err);
    }
});
```

#### Step-by-Step Breakdown

**Step 1: Get LaTeX String**
```javascript
let latex = MathEditor.mathField.latex();
// Example: "\\frac{1}{2}+\\sqrt{4}"
```

**Step 2: Variable Detection**
```javascript
const cleaned = latex.replace(/\\[a-zA-Z]+/g, "");
// Removes: \frac, \sqrt, \pi, etc.
// Example: "\\frac{x}{2}" → "{x}{2}"

if (/[a-zA-Z]/.test(cleaned)) {
    alert("Expression contains variables. Cannot evaluate.");
    return;
}
```

**Why This Check?**
- LaTeX uses letters for commands (`\frac`) AND variables (`x`, `y`)
- After removing commands, remaining letters are variables
- Can't evaluate expressions with undefined variables
- Example:
  - `\frac{1}{2}` → `{1}{2}` → No letters ✓
  - `\frac{x}{2}` → `{x}{2}` → Has 'x' ✗

**Step 3: LaTeX → Math.js Conversion**

**Fractions:**
```javascript
.replace(/\\frac{([^}]*)}{([^}]*)}/g, "($1)/($2)")
```
- **Regex Breakdown:**
  - `\\frac` - Literal "\frac"
  - `{([^}]*)}` - Capture group: anything except `}`
  - `{([^}]*)}` - Second capture group
  - `g` - Global flag (replace all occurrences)
- **Transformation:**
  - `\frac{1}{2}` → `(1)/(2)`
  - `\frac{3+4}{5-1}` → `(3+4)/(5-1)`
- **Why Parentheses?**
  - Preserve order of operations
  - `3+4/5-1` is different from `(3+4)/(5-1)`

**Square Roots:**
```javascript
.replace(/\\sqrt{([^}]*)}/g, "sqrt($1)")
```
- `\sqrt{9}` → `sqrt(9)`
- `\sqrt{16+9}` → `sqrt(16+9)`

**Multiplication:**
```javascript
.replace(/\\cdot/g, "*")
```
- `3\\cdot 4` → `3*4`
- LaTeX uses `\cdot` for ·, Math.js uses `*`

**Pi:**
```javascript
.replace(/\\pi/g, "pi")
```
- `\\pi` → `pi`
- Math.js has built-in `pi` constant

**Parentheses:**
```javascript
.replace(/\\left|\\right/g, "");
```
- Removes `\left` and `\right` commands
- LaTeX uses these for sizing, Math.js doesn't need them
- `\left(x\right)` → `(x)`

#### Conversion Examples

| LaTeX Input | Cleaned | Math.js Expression | Result |
|-------------|---------|-------------------|--------|
| `\frac{1}{2}` | `{1}{2}` | `(1)/(2)` | `0.5` |
| `\sqrt{16}` | `{16}` | `sqrt(16)` | `4` |
| `2\cdot\pi` | `2{}\pi` | `2*pi` | `6.283...` |
| `\frac{1}{2}+\sqrt{4}` | `{1}{2}+{4}` | `(1)/(2)+sqrt(4)` | `2.5` |
| `\frac{x}{2}` | `{x}{2}` | ⚠️ Variable detected | Error |

**Step 4: Evaluation**
```javascript
const result = math.evaluate(expr);
```
- Math.js parses and evaluates the expression
- Handles operator precedence automatically
- Returns numeric result

**Step 5: Display Result**
```javascript
MathEditor.mathField.latex(result.toString());
```
- Converts number to string
- Sets the MathQuill field to show result
- Replaces original expression with answer
- Example: `0.5` displays as `0.5`

**Step 6: Error Handling**
```javascript
catch (err) {
    alert("Invalid numeric expression");
    console.error(err);
}
```
- Catches Math.js evaluation errors
- Shows user-friendly alert
- Logs technical error to console for debugging

### Complete Calculation Flow Example

**User Input:** `1/2 + √(4)`

```
Step 1: MathQuill Input
  User types: 1, /, 2, +, clicks √ button, types 4
  Internal LaTeX: \frac{1}{2}+\sqrt{4}

Step 2: Variable Check
  Remove commands: {1}{2}+{4}
  Check for letters: None found ✓

Step 3: Convert to Math.js
  \frac{1}{2} → (1)/(2)
  \sqrt{4} → sqrt(4)
  Result: (1)/(2)+sqrt(4)

Step 4: Evaluate
  (1)/(2) = 0.5
  sqrt(4) = 2
  0.5 + 2 = 2.5

Step 5: Display
  MathQuill field now shows: 2.5
  
Step 6: Auto-update
  updateOutputs() called
  LaTeX output: 2.5
  Rendered output: 2.5
```

---

## Advanced Features Explained

### 1. Real-Time Rendering

```javascript
const mathField = MQ.MathField(editorSpan, {
    handlers: {
        edit: updateOutputs  // Called on EVERY keystroke
    }
});
```

**How It Works:**
- User types or clicks button
- MathQuill triggers `edit` event
- `updateOutputs()` called automatically
- LaTeX extracted and rendered
- No "Update" button needed

**Performance:**
- KaTeX is fast enough for real-time rendering
- MathJax would be slower
- Debouncing not needed for typical use

### 2. Cursor Management

MathQuill handles cursor positioning automatically:

```
In \frac{}{}, cursor flow:
1. Start: \frac{█}{}
2. Type "1": \frac{1█}{}
3. Space/Tab: \frac{1}{█}
4. Type "2": \frac{1}{2█}
5. Space/Tab: \frac{1}{2}█
```

**Navigation Keys:**
- **Arrow keys**: Move cursor
- **Tab/Space**: Jump between fields
- **Backspace**: Smart deletion (removes whole structures)
- **Escape**: Exit current field

### 3. LaTeX Template System

```javascript
data-latex="\\frac{}{}"
```

**Template Structure:**
- `{}` indicates empty fields
- Cursor automatically placed in first `{}`
- Tab/Space moves to next `{}`

**Examples:**

| Button | Template | Cursor Start | After First Input |
|--------|----------|--------------|-------------------|
| Fraction | `\frac{}{}` | `\frac{█}{}` | `\frac{5█}{}` |
| Square Root | `\sqrt{}` | `\sqrt{█}` | `\sqrt{9█}` |
| Power | `^{}` | `x^{█}` | `x^{2█}` |
| Subscript | `_{}` | `x_{█}` | `x_{i█}` |
| Sum | `\sum_{}^{}` | `\sum_{█}^{}` | `\sum_{n█}^{}` |

### 4. Error Recovery

**Graceful Degradation:**

```javascript
try {
    katex.render(latex, renderOutput, {
        throwOnError: false  // Don't throw, show error inline
    });
} catch (e) {
    renderOutput.textContent = "Rendering error";
}
```

**Layers of Protection:**
1. KaTeX's `throwOnError: false` handles most invalid LaTeX
2. Try-catch handles catastrophic errors
3. User always sees feedback, never blank screen

**Variable Detection:**
```javascript
if (/[a-zA-Z]/.test(cleaned)) {
    alert("Expression contains variables. Cannot evaluate.");
    return;
}
```
- Prevents Math.js errors from undefined variables
- Clear error message
- Fails fast before evaluation

---

## Comparison: Custom Editor vs. MathQuill

### Your Previous Calculator

| Aspect | Your Custom Editor | MathQuill Editor |
|--------|-------------------|------------------|
| **Implementation** | 600+ lines custom JS | ~100 lines with library |
| **Input Method** | Type keywords + auto-detect | WYSIWYG visual editing |
| **Cursor Control** | Manual selection API | Automatic |
| **Math Rendering** | Custom HTML/CSS | Standard LaTeX |
| **Browser Support** | Requires contenteditable | Works everywhere |
| **Learning Curve** | Simple text input | LaTeX knowledge helpful |
| **Extensibility** | Add custom keywords | Limited to LaTeX |
| **Accessibility** | Custom implementation | Built-in |

### When to Use Each

**Use Custom Editor When:**
- Learning exercise
- Full control needed
- Custom notation required
- Lightweight solution
- No dependencies desired

**Use MathQuill When:**
- Production application
- Standard math notation
- Professional appearance
- Complex expressions
- Time constraints

---

## Technical Deep Dives

### MathQuill API

**Key Methods:**

```javascript
const MQ = MathQuill.getInterface(2);  // Get API version 2

// Create editable field
const mathField = MQ.MathField(element, config);

// Writing content
mathField.write('x^2');        // Insert LaTeX
mathField.latex('\\frac{1}{2}'); // Set entire content

// Reading content
const latex = mathField.latex(); // Get LaTeX string
const text = mathField.text();   // Get plain text

// Control
mathField.focus();               // Focus field
mathField.blur();                // Unfocus
mathField.select();              // Select all
mathField.clearSelection();      // Clear selection
```

**Configuration Options:**

```javascript
{
    // Keyboard behavior
    spaceBehavesLikeTab: true,        // Space acts as Tab
    leftRightIntoCmdGoes: 'up',       // Arrow key behavior
    restrictMismatchedBrackets: true, // Prevent mismatched ()
    
    // Event handlers
    handlers: {
        edit: function() { },          // On any edit
        enter: function() { },         // On Enter key
        upOutOf: function() { },       // Arrow up at top
        downOutOf: function() { }      // Arrow down at bottom
    },
    
    // Visual
    substituteTextarea: function() { }, // Custom textarea
    sumStartsWithNEquals: true         // \sum starts with n=
}
```

### KaTeX Rendering

**Basic Usage:**
```javascript
katex.render(
    latexString,        // LaTeX to render
    domElement,         // Where to render
    {
        throwOnError: false,     // Don't crash on errors
        displayMode: true,       // Block vs inline
        output: 'html',          // 'html' or 'mathml'
        trust: false,            // Allow \href, \includegraphics
        strict: 'warn',          // Error level
        macros: {                // Custom commands
            "\\RR": "\\mathbb{R}"
        }
    }
);
```

**Performance Comparison:**

| Library | Render Time | File Size | Features |
|---------|-------------|-----------|----------|
| KaTeX | ~3ms | 280KB | Fast, subset |
| MathJax | ~30ms | 540KB | Slow, complete |
| Custom CSS | N/A | Small | Limited |

### Math.js Capabilities

**Basic Operations:**
```javascript
math.evaluate('2 + 3');              // 5
math.evaluate('2 * 3^2');            // 18
math.evaluate('sqrt(16)');           // 4
math.evaluate('sin(pi/2)');          // 1
```

**Complex Numbers:**
```javascript
math.evaluate('sqrt(-4)');           // 2i
math.evaluate('i * i');              // -1
```

**Units:**
```javascript
math.evaluate('5 cm + 2 inch');      // 7.08 cm
math.evaluate('3 kg * 2 m/s^2');     // 6 N
```

**Matrices:**
```javascript
math.evaluate('[[1,2],[3,4]] * [[5,6],[7,8]]');
// [[19, 22], [43, 50]]
```

**Variables:**
```javascript
const scope = { x: 3, y: 4 };
math.evaluate('x^2 + y^2', scope);   // 25
```

---

## Security Considerations

### XSS Prevention

**Problem:**
User input could contain malicious code.

**Protection Layers:**

1. **MathQuill Sanitization:**
   ```javascript
   mathField.latex(); // Already sanitized by MathQuill
   ```

2. **KaTeX Escaping:**
   ```javascript
   katex.render(latex, element, {
       trust: false  // Disables \href and other dangerous commands
   });
   ```

3. **Math.js Safety:**
   ```javascript
   math.evaluate(expr); // Doesn't use eval(), safe sandbox
   ```

### LaTeX Injection

**Example Attack:**
```latex
\href{javascript:alert('XSS')}{\text{Click me}}
```

**Defense:**
```javascript
{
    trust: false  // KaTeX ignores \href
}
```

### Regular Expression Safety

**ReDoS (Regular Expression Denial of Service):**

Potentially vulnerable:
```javascript
.replace(/\\frac{([^}]*)}{([^}]*)}/g, "($1)/($2)")
```

**Why It's Safe Here:**
- Input is controlled (from MathQuill)
- Pattern is simple (no catastrophic backtracking)
- Expression length limited by UI

---

## Optimization Opportunities

### 1. Event Delegation

**Current:**
```javascript
document.querySelectorAll(".toolbar button").forEach(btn => {
    btn.addEventListener("click", handler);
});
```

**Optimized:**
```javascript
document.querySelector(".toolbar").addEventListener("click", (e) => {
    if (e.target.matches("button[data-latex]")) {
        MathEditor.insertLatex(e.target.dataset.latex);
    }
});
```
- One listener instead of 12
- Handles dynamically added buttons
- Slightly better memory usage

### 2. Debouncing Updates

**Current:**
```javascript
handlers: {
    edit: updateOutputs  // Called on EVERY keystroke
}
```

**Optimized:**
```javascript
function debounce(func, wait) {
    let timeout;
    return function(...args) {
        clearTimeout(timeout);
        timeout = setTimeout(() => func.apply(this, args), wait);
    };
}

handlers: {
    edit: debounce(updateOutputs, 100)  // Wait 100ms
}
```
- Reduces rendering calls
- Smoother typing experience
- Only needed for very complex expressions

### 3. Caching Regex Patterns

**Current:**
```javascript
.replace(/\\frac{([^}]*)}{([^}]*)}/g, "($1)/($2)")
```

**Optimized:**
```javascript
const FRAC_REGEX = /\\frac{([^}]*)}{([^}]*)}/g;
.replace(FRAC_REGEX, "($1)/($2)")
```
- Regex compiled once
- Reused across invocations
- Minimal performance gain in practice

---

## Extension Ideas

### 1. History System

```javascript
const history = [];
let historyIndex = -1;

function saveState() {
    const latex = mathField.latex();
    history.push(latex);
    historyIndex = history.length - 1;
}

function undo() {
    if (historyIndex > 0) {
        historyIndex--;
        mathField.latex(history[historyIndex]);
    }
}

function redo() {
    if (historyIndex < history.length - 1) {
        historyIndex++;
        mathField.latex(history[historyIndex]);
    }
}

// Save on edit
handlers: {
    edit: () => {
        updateOutputs();
        saveState();
    }
}
```

### 2. Copy LaTeX to Clipboard

```javascript
function copyLatex() {
    const latex = mathField.latex();
    navigator.clipboard.writeText(latex)
        .then(() => alert('LaTeX copied!'))
        .catch(err => console.error('Copy failed:', err));
}

// Add button
<button onclick="copyLatex()">Copy LaTeX</button>
```

### 3. Save/Load Expressions

```javascript
function saveExpression() {
    const latex = mathField.latex();
    localStorage.setItem('savedExpression', latex);
    alert('Expression saved!');
}

function loadExpression() {
    const latex = localStorage.getItem('savedExpression');
    if (latex) {
        mathField.latex(latex);
    }
}
```

### 4. Export to Image

```javascript
function exportToPNG() {
    const element = document.getElementById('render-output');
    html2canvas(element).then(canvas => {
        const link = document.createElement('a');
        link.download = 'equation.png';
        link.href = canvas.toDataURL();
        link.click();
    });
}

// Requires html2canvas library
<script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
```

### 5. Multi-Step Calculations

```javascript
const steps = [];

function calculate() {
    const latex = mathField.latex();
    const result = evaluateExpression(latex);
    
    steps.push({
        expression: latex,
        result: result
    });
    
    displaySteps();
}

function displaySteps() {
    const stepsDiv = document.getElementById('steps');
    stepsDiv.innerHTML = steps.map((step, i) => `
        <div>
            Step ${i+1}: ${step.expression} = ${step.result}
        </div>
    `).join('');
}
```

### 6. Advanced Calculator Features

```javascript
// Variables
const variables = {};

function setVariable(name, value) {
    variables[name] = value;
}

function evaluateWithVariables(expr) {
    return math.evaluate(expr, variables);
}

// Example usage:
setVariable('x', 5);
evaluateWithVariables('2*x + 3'); // 13
```

---

## Troubleshooting Guide

### Common Issues

**1. MathQuill Not Loading**

**Symptom:** Blank input field, console error
```
MathQuill is not defined
```

**Solution:**
- Check jQuery loaded before MathQuill
- Verify CDN URLs accessible
- Check browser console for 404 errors

**2. KaTeX Rendering Fails**

**Symptom:** "Rendering error" displayed

**Solution:**
```javascript
// Add detailed error logging
try {
    katex.render(latex, renderOutput, { throwOnError: false });
} catch (e) {
    console.log('KaTeX Error:', e.message);
    console.log('LaTeX:', latex);
    renderOutput.textContent = "Rendering error: " + e.message;
}
```

**3. Calculator Returns Undefined**

**Symptom:** Result is `undefined`

**Solution:**
- Check LaTeX → Math.js conversion
- Add logging:
```javascript
console.log('Original LaTeX:', latex);
console.log('Converted expr:', expr);
console.log('Result:', result);
```

**4. Variables Not Detected**

**Symptom:** Calculator tries to evaluate `x+1`

**Solution:**
- Check regex is working:
```javascript
const cleaned = latex.replace(/\\[a-zA-Z]+/g, "");
console.log('Cleaned:', cleaned);
console.log('Has letters?', /[a-zA-Z]/.test(cleaned));
```

---

## Best Practices

### 1. Error Messages

**Bad:**
```javascript
alert('Error');
```

**Good:**
```javascript
alert('Expression contains variables. Cannot evaluate.');
```

**Better:**
```javascript
alert('Expression contains variables (x, y, etc.). Please use only numbers.');
```

### 2. User Feedback

**Visual indicators:**
```css
.calculating {
    opacity: 0.5;
    pointer-events: none;
}
```

```javascript
calcBtn.classList.add('calculating');
setTimeout(() => {
    const result = calculate();
    calcBtn.classList.remove('calculating');
}, 0);
```

### 3. Keyboard Shortcuts

```javascript
document.addEventListener('keydown', (e) => {
    // Ctrl+Enter to calculate
    if (e.ctrlKey && e.key === 'Enter') {
        e.preventDefault();
        calculate();
    }
    
    // Ctrl+Z for undo
    if (e.ctrlKey && e.key === 'z') {
        e.preventDefault();
        undo();
    }
});
```

### 4. Mobile Responsiveness

```css
@media (max-width: 768px) {
    .container {
        width: 100%;
        padding: 10px;
    }
    
    .toolbar {
        grid-template-columns: repeat(3, 1fr);
    }
    
    button {
        padding: 10px;
        font-size: 14px;
    }
}
```

---

## Performance Metrics

### Library Load Times

| Library | Size | Load Time (3G) | Load Time (4G) |
|---------|------|----------------|----------------|
| jQuery 3.7.1 | 87 KB | ~300ms | ~100ms |
| MathQuill | 65 KB | ~220ms | ~75ms |
| KaTeX | 280 KB | ~950ms | ~320ms |
| Math.js | 520 KB | ~1.8s | ~600ms |
| **Total** | **952 KB** | **~3.3s** | **~1.1s** |

### Optimization: Load Only What's Needed

**Option 1: Custom MathQuill Build**
- Remove unused features
- Size: ~40 KB (40% smaller)

**Option 2: Math.js Core**
```javascript
<script src="https://cdn.jsdelivr.net/npm/mathjs@12/lib/browser/math.core.js"></script>
```
- Basic operations only
- Size: ~180 KB (65% smaller)

**Option 3: Lazy Loading**
```javascript
// Load Math.js only when calculate button clicked
let mathJsLoaded = false;

calcBtn.addEventListener('click', async () => {
    if (!mathJsLoaded) {
        await loadScript('https://cdn.jsdelivr.net/npm/mathjs@12/lib/browser/math.js');
        mathJsLoaded = true;
    }
    calculate();
});
```

---

## Summary

### Key Takeaways

1. **MathQuill provides professional math input** with minimal code
2. **KaTeX handles rendering** fast and reliably
3. **Math.js evaluates expressions** safely without `eval()`
4. **LaTeX is the lingua franca** for mathematical notation
5. **Module pattern organizes code** cleanly

### Architecture Comparison

**Your Custom Editor:**
- Full control
- Custom notation
- Educational value
- More code to maintain

**MathQuill Editor:**
- Professional appearance
- Standard LaTeX
- Less code
- Dependency on libraries

### When to Choose Which

**Use Custom Editor for:**
- Learning projects
- Custom math notation
- Embedded systems
- Offline-first applications

**Use MathQuill for:**
- Production websites
- Academic tools
- Professional applications
- Standard mathematical notation

Both approaches have merit - the custom editor teaches fundamentals, while MathQuill provides a production-ready solution. Understanding both makes you a better developer!