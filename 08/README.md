# Scientific Math Input 🔢

A sophisticated, interactive web-based mathematical expression editor with real-time visual formatting and evaluation capabilities. This tool allows users to type mathematical expressions naturally and see them formatted beautifully in real-time, just like in professional mathematical typesetting software.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Version](https://img.shields.io/badge/version-1.0.0-green.svg)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-yellow.svg)

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Demo & Screenshots](#demo--screenshots)
- [How It Works](#how-it-works)
- [Installation](#installation)
- [Usage Guide](#usage-guide)
- [Technical Architecture](#technical-architecture)
- [File Structure](#file-structure)
- [API Reference](#api-reference)
- [Customization](#customization)
- [Browser Compatibility](#browser-compatibility)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

Scientific Math Input is a modern, intuitive mathematical expression editor that bridges the gap between plain text input and professional mathematical notation. It provides an interactive contenteditable interface that automatically transforms typed mathematical syntax into beautifully formatted expressions with square roots, fractions, superscripts, functions, and more.

### What Makes It Special?

- **Zero Configuration**: No external mathematical rendering libraries required (like MathJax or KaTeX)
- **Real-time Conversion**: Instant transformation from text to visual mathematical notation
- **Smart Auto-Detection**: Automatically recognizes mathematical keywords and symbols
- **Mode-Based Editing**: Intelligent cursor management for complex nested expressions
- **Math.js Integration**: Powerful expression evaluation with comprehensive mathematical functions

---

## Features

### Core Capabilities

#### 🎯 **Auto-Formatting**
Type naturally and watch your expressions transform:
- Type `sqrt` → Converts to √ symbol with overline
- Type `sin` → Creates sin() function with cursor inside parentheses
- Type `^` → Enters superscript mode (e.g., x² or e^πi)
- Type `/` → Creates visual fraction with numerator/denominator
- Type `abs` → Creates absolute value bars |x|

#### 📐 **Mathematical Elements**

**Square Roots**
```
Input: sqrt
Visual: √‾‾‾
Format: Custom SVG radical with overline border
```

**Fractions**
```
Input: 3/4
Visual: 3
       ─
       4
Format: Inline vertical layout with fraction line
```

**Superscripts**
```
Input: x^2
Visual: x²
Format: Smaller, elevated text
```

**Functions**
```
Supported: sin, cos, tan, asin, acos, atan, log, ln, exp, ceil, floor, round
Visual: sin(x) with function name highlighted
```

**Absolute Values**
```
Input: abs
Visual: |x|
Format: Bordered content with vertical bars
```

#### ⌨️ **Smart Keyboard Navigation**

| Key | Action |
|-----|--------|
| **Arrow Right** | Exit special modes (sqrt, superscript, function, abs) |
| **Arrow Left** | Navigate between numerator/denominator in fractions |
| **Space / Tab / Enter** | Exit current mode and return to normal typing |
| **Escape** | Cancel current mode |
| **Backspace** | Smart deletion that respects mode boundaries |

#### 🔘 **Button Palette**

24 quick-insert buttons organized in a grid:
- **Mathematical Operations**: √, x^n, (a/b), |x|, π, e
- **Trigonometric Functions**: sin, cos, tan, asin, acos, atan
- **Logarithmic & Other Functions**: log, ln, exp, ceil, floor, round
- **Operators & Symbols**: ( ) + − × =

#### 🧮 **Expression Evaluation**

Powered by Math.js for accurate computation:
- Arithmetic operations
- Trigonometric functions (in radians)
- Logarithmic functions
- Exponential functions
- Constants (π, e)
- Complex nested expressions

#### 🎨 **Visual Design**

- **Modern Gradient UI**: Dark blue gradient background with clean white container
- **Custom Typography**: JetBrains Mono for code and Crimson Pro for headings
- **Smooth Animations**: Hover effects, transitions, and pulse indicators
- **Responsive Layout**: Adapts to different screen sizes
- **Real-time Preview**: See both formatted and raw expressions simultaneously

---

## Demo & Screenshots

### Main Interface

The application features a large, responsive input area with:
- Visual mathematical display (top)
- Raw expression preview (below input)
- Button palette for quick symbol insertion
- Calculate button for evaluation
- Result display section

### Example Transformations

**Before Typing**: Empty editor with placeholder

**After Typing "sqrt(x^2 + y^2)"**:
```
Visual Display: √‾‾‾‾‾‾‾‾‾‾
                 x² + y²

Raw Expression: sqrt(x^2 + y^2)
```

**After Typing "sin(π/2)"**:
```
Visual Display: sin(π/2)

Raw Expression: sin(pi/2)

Result: 1
```

---

## How It Works

### The Core Concept

The editor uses a **hybrid rendering approach**:

1. **Data Model**: All expressions are stored in a JavaScript array (`expressionData`) as structured objects
2. **Visual Rendering**: HTML/CSS creates the visual representation
3. **Mode Management**: A state machine tracks what the user is currently editing
4. **Real-time Sync**: Every keystroke updates both the visual display and raw expression

### Mode-Based Architecture

The editor operates in different "modes" depending on what mathematical element you're editing:

```javascript
Modes:
- normal: Regular text input
- sqrt: Inside a square root
- superscript: Typing an exponent
- fraction-num: Editing fraction numerator
- fraction-den: Editing fraction denominator
- function: Inside function parentheses
- abs: Inside absolute value bars
```

### Expression Data Structure

Expressions are stored as an array of typed objects:

```javascript
expressionData = [
  { type: 'text', text: '2 + ' },
  { type: 'sqrt', data: { content: '4' } },
  { type: 'text', text: ' × ' },
  { type: 'superscript', data: { base: 'x', exponent: '2' } }
]
```

This structure is then rendered to HTML and converted to a raw expression for evaluation.

---

## Installation

### Quick Start

1. **Clone or Download** the repository:
```bash
git clone https://github.com/yourusername/scientific-math-input.git
cd scientific-math-input
```

2. **Open in Browser**:
```bash
# Simply open index.html in any modern web browser
open index.html
# Or on Windows
start index.html
# Or on Linux
xdg-open index.html
```

That's it! No build process, no dependencies to install.

### Project Structure

```
scientific-math-input/
│
├── index.html          # Main HTML structure
├── styles.css          # All styling and animations
├── script.js           # Core JavaScript logic
└── README.md           # This file
```

### CDN Dependencies

The project uses these external libraries via CDN:

- **Math.js** (v11.11.0): Mathematical expression evaluation
  ```html
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.11.0/math.min.js"></script>
  ```

- **Google Fonts**: JetBrains Mono & Crimson Pro
  ```html
  <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600&family=Crimson+Pro:wght@600;700&display=swap" rel="stylesheet">
  ```

---

## Usage Guide

### Basic Usage

#### 1. **Start Typing**
Simply click in the math display area and start typing your expression. The editor auto-detects mathematical keywords.

#### 2. **Use Auto-Formatting**
Type these keywords to trigger auto-formatting:
- `sqrt` → Square root
- `sin`, `cos`, `tan`, etc. → Trig functions
- `abs` → Absolute value
- `^` → Superscript
- `/` → Fraction

#### 3. **Navigate with Keyboard**
- Use **Arrow Right**, **Space**, **Tab**, or **Enter** to exit special modes
- Use **Backspace** to delete characters (smart deletion respects boundaries)
- Use **Escape** to cancel the current mode

#### 4. **Evaluate**
Click the **"Calculate Result"** button to evaluate your expression.

### Advanced Examples

#### Example 1: Quadratic Formula
```
Type: (-b + sqrt(b^2 - 4ac)) / 2a

Visual Result: 
       -b + √‾‾‾‾‾‾‾‾‾‾‾
            b² - 4ac
       ─────────────────
              2a
```

#### Example 2: Trigonometric Expression
```
Type: sin(π/4) + cos(π/4)

Result: 1.414213562 (approximately √2)
```

#### Example 3: Complex Nested Expression
```
Type: sqrt(sin(x)^2 + cos(x)^2)

This evaluates to 1 for any x (Pythagorean identity)
```

### Tips & Tricks

**Tip 1: Quick Symbol Insertion**
Use the button palette for symbols you don't want to type (π, ×, √, etc.)

**Tip 2: Avoid Nested Fractions**
In sqrt, function, and abs modes, the `/` character is treated as text, not a fraction trigger. This prevents accidental nested fractions.

**Tip 3: Function Name Priority**
Type longer function names first. The editor checks for `asin` before `sin`, so typing "asin" won't accidentally trigger "sin" mode.

**Tip 4: Raw Expression View**
The gray box below the main input shows the actual mathematical syntax that will be evaluated. Use this to verify your expression.

**Tip 5: Mode Indicator**
Watch the top-right corner of the input area for the current mode indicator. It pulses when you're in a special mode.

---

## Technical Architecture

### 1. Event Handling System

The editor uses the `beforeinput` event to intercept every keystroke:

```javascript
mathDisplay.addEventListener('beforeinput', function(e) {
    e.preventDefault(); // Prevent default contenteditable behavior
    
    // Process the input character
    const data = e.data;
    
    // Handle special characters
    if (data === '^') enterSuperscriptMode();
    else if (data === '/') enterFractionMode();
    else insertCharacter(data);
});
```

### 2. Character Insertion Logic

When a character is typed, the system determines where it should go based on the current mode:

```javascript
function insertCharacter(char) {
    if (currentMode === 'sqrt') {
        currentSqrt.content += char; // Add to sqrt content
    } else if (currentMode === 'superscript') {
        currentSuperscript.exponent += char; // Add to exponent
    } else if (currentMode === 'fraction-den') {
        currentFraction.denominator += char; // Add to denominator
    } else {
        // Normal mode: add to or create text node
        // Also check for keyword triggers (sqrt, sin, abs, etc.)
    }
    render(); // Re-render the display
}
```

### 3. Keyword Detection

The editor scans text as you type to detect mathematical keywords:

```javascript
// Check if text ends with 'sqrt'
if (lastText.text.endsWith('sqrt')) {
    lastText.text = lastText.text.slice(0, -4); // Remove 'sqrt'
    enterSqrtMode(); // Enter square root mode
}

// Check for functions (longer names first to avoid partial matches)
const functions = ['asin', 'acos', 'atan', 'sin', 'cos', 'tan', ...];
for (let func of functions) {
    if (lastText.text.endsWith(func)) {
        // Verify it's not part of a larger word
        if (prefix === '' || !/[a-zA-Z]/.test(prefix[prefix.length - 1])) {
            enterFunctionMode(func);
        }
    }
}
```

### 4. Rendering System

The render function converts the data model to HTML:

```javascript
function render() {
    let html = '';
    for (let elem of expressionData) {
        if (elem.type === 'text') {
            html += `<span>${escapeHtml(elem.text)}</span>`;
        } else if (elem.type === 'sqrt') {
            html += `<span class="sqrt-symbol">
                <svg class="sqrt-radical">...</svg>
                <span class="sqrt-content">${elem.data.content}</span>
            </span>`;
        }
        // ... handle other types
    }
    mathDisplay.innerHTML = html;
    
    // Position cursor correctly
    updateCursor();
}
```

### 5. Cursor Management

One of the most complex parts is keeping the cursor in the right place:

```javascript
// Use a special ID to mark where the cursor should be
html += `<span id="cursorTarget">${content}</span>`;

// After rendering, find and focus the cursor target
const cursorTarget = document.getElementById('cursorTarget');
if (cursorTarget) {
    range.setStart(cursorTarget.firstChild, cursorTarget.textContent.length);
    selection.addRange(range);
}
```

### 6. Backspace Handling

Smart deletion removes characters while respecting mode boundaries:

```javascript
function handleBackspace() {
    if (currentMode === 'sqrt' && currentSqrt.content) {
        currentSqrt.content = currentSqrt.content.slice(0, -1);
    } else if (currentMode === 'sqrt' && !currentSqrt.content) {
        // Empty sqrt - remove it entirely and exit mode
        expressionData.pop();
        exitSqrtMode();
    }
    // ... similar logic for other modes
    render();
}
```

### 7. Expression Evaluation

The raw expression is built from the data model:

```javascript
function updateRawExpression() {
    let raw = '';
    for (let elem of expressionData) {
        if (elem.type === 'sqrt') {
            raw += `sqrt(${elem.data.content})`;
        } else if (elem.type === 'superscript') {
            raw += `${elem.data.base}^${elem.data.exponent}`;
        } else if (elem.type === 'fraction') {
            raw += `(${elem.data.numerator})/(${elem.data.denominator})`;
        }
        // ... handle other types
    }
    return raw;
}
```

Then evaluated using Math.js:

```javascript
function evaluateExpression() {
    const expression = rawExpression.textContent.trim();
    const processed = expression.replace(/π/g, 'pi'); // Convert π to 'pi'
    const result = math.evaluate(processed);
    displayResult(result);
}
```

---

## File Structure

### index.html

**Purpose**: Main HTML structure and layout

**Key Sections**:
- `<head>`: Meta tags, external dependencies (Math.js, fonts)
- `.container`: Main wrapper for the entire application
- `#mathDisplay`: Contenteditable div for expression input
- `#rawExpression`: Read-only display of raw expression
- `.button-grid`: 6×4 grid of mathematical symbol buttons
- `.result-section`: Result display area
- `.info-box`: Usage instructions

**Notable Features**:
- Uses `contenteditable="true"` for rich text editing
- `spellcheck="false"` to avoid red underlines in math expressions
- Semantic HTML structure for accessibility

### styles.css

**Purpose**: All visual styling and animations

**Major Sections**:

1. **Global Styles**
   - CSS reset
   - Body gradient background
   - Font family definitions

2. **Container & Typography**
   - Main container styling
   - Heading styles (Crimson Pro font)
   - Subtitle and label styles

3. **Math Display**
   - Contenteditable input area
   - Focus states and transitions
   - Placeholder text styling

4. **Mathematical Elements**
   ```css
   .superscript { /* Elevated, smaller text */ }
   .fraction { /* Vertical flex layout */ }
   .sqrt-symbol { /* SVG radical + content */ }
   .abs-bars { /* Bordered content */ }
   .function-name { /* Colored function text */ }
   ```

5. **Button Styles**
   - Grid layout
   - Hover effects (gradient shift, elevation)
   - Active states

6. **Result Section**
   - Success state (green gradient)
   - Error state (red gradient)
   - Conditional display

7. **Utility Classes**
   - Mode indicator (pulsing animation)
   - Raw expression view
   - Info box

### script.js

**Purpose**: Core application logic and interactivity

**Major Components**:

1. **State Management**
   ```javascript
   let expressionData = [];      // Array of expression elements
   let currentMode = 'normal';   // Current editing mode
   let currentFraction = null;   // Active fraction object
   let currentSuperscript = null;// Active superscript object
   // ... etc
   ```

2. **Event Listeners**
   - `beforeinput`: Intercepts all text input
   - `paste`: Handles clipboard paste
   - `keydown`: Navigation and special keys

3. **Mode Management Functions**
   - `enterSuperscriptMode()`
   - `enterFractionMode()`
   - `enterSqrtMode()`
   - `enterFunctionMode(funcName)`
   - `enterAbsMode()`
   - Exit functions for each mode

4. **Input Processing**
   - `insertCharacter(char)`: Adds character to appropriate location
   - `handleBackspace()`: Smart deletion logic
   - Keyword detection for auto-formatting

5. **Rendering**
   - `render()`: Converts data model to HTML
   - `updateRawExpression()`: Builds evaluatable string
   - `renderInlineContent()`: Handles nested fractions in content

6. **Evaluation**
   - `evaluateExpression()`: Uses Math.js to compute result
   - Error handling and display

7. **UI Helpers**
   - `insertText()`: Button click handler
   - `insertSymbol()`: Special symbol insertion
   - `insertFunction()`: Function insertion
   - `escapeHtml()`: Security against XSS

---

## API Reference

### Global Variables

```javascript
expressionData: Array<Object>
// Array storing all expression elements in sequence
// Each object has a 'type' and type-specific data

currentMode: String
// Current editing mode: 'normal', 'superscript', 'fraction-num', 
// 'fraction-den', 'sqrt', 'function', 'abs'

currentFraction: Object | null
// { numerator: String, denominator: String }

currentSuperscript: Object | null
// { base: String, exponent: String }

currentSqrt: Object | null
// { content: String }

currentFunction: Object | null
// { name: String, content: String }

currentAbs: Object | null
// { content: String }
```

### Core Functions

#### insertCharacter(char)
Inserts a character at the current cursor position based on active mode.

**Parameters**:
- `char` (String): Single character to insert

**Behavior**:
- Appends to appropriate location based on `currentMode`
- Triggers keyword detection in normal mode
- Calls `render()` after insertion

#### enterSuperscriptMode()
Switches to superscript mode, using previous text as base.

**Effects**:
- Grabs last text element as base
- Creates new superscript object
- Updates mode indicator
- Calls `render()`

#### enterFractionMode()
Switches to fraction mode with denominator editing.

**Effects**:
- Uses last text as numerator
- Creates new fraction object
- Sets mode to 'fraction-den'
- Updates mode indicator

#### render()
Converts expressionData to HTML and updates display.

**Process**:
1. Iterates through expressionData
2. Generates HTML for each element type
3. Updates mathDisplay.innerHTML
4. Adjusts sqrt radical heights
5. Updates raw expression
6. Positions cursor correctly

#### evaluateExpression()
Evaluates the mathematical expression and displays result.

**Process**:
1. Gets raw expression
2. Replaces π with 'pi'
3. Uses math.evaluate()
4. Formats result (rounds to 10 decimal places)
5. Displays in result section
6. Handles errors gracefully

### Button Interface Functions

#### insertText(text)
Inserts a string of text character by character.

**Parameters**:
- `text` (String): Text to insert

#### insertSymbol(type)
Inserts a special mathematical symbol.

**Parameters**:
- `type` (String): One of 'sqrt', 'abs', 'power', 'fraction'

#### insertFunction(funcName)
Creates a function call with parentheses.

**Parameters**:
- `funcName` (String): Function name (sin, cos, log, etc.)

---

## Customization

### Adding New Functions

To add support for a new mathematical function:

1. **Update the function list** in `script.js`:
```javascript
const functions = ['asin', 'acos', 'atan', 'sin', 'cos', 'tan', 
                   'log', 'ln', 'exp', 'ceil', 'floor', 'round', 
                   'yourNewFunction']; // Add here
```

2. **Add a button** in `index.html`:
```html
<button class="math-btn" onclick="insertFunction('yourNewFunction')">
    yourFn
</button>
```

3. **Ensure Math.js supports it** (most standard functions are supported)

### Styling Customization

#### Change Color Scheme

In `styles.css`, modify these variables:

```css
/* Background gradient */
body {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
}

/* Primary accent color */
.math-btn:hover {
    background: linear-gradient(135deg, #0f3460 0%, #16213e 100%);
}

/* Success result color */
.result-section {
    background: linear-gradient(135deg, #e8f5e9 0%, #c8e6c9 100%);
    border-left: 6px solid #4caf50;
}
```

#### Change Fonts

Replace the Google Fonts import in `index.html`:

```html
<link href="https://fonts.googleapis.com/css2?family=YourFont&display=swap" rel="stylesheet">
```

Then update the CSS:

```css
body {
    font-family: 'YourFont', monospace;
}
```

#### Adjust Button Layout

Change the grid columns in `styles.css`:

```css
.button-grid {
    grid-template-columns: repeat(6, 1fr); /* Change 6 to desired columns */
    gap: 12px;
}
```

### Adding New Mathematical Elements

To add a completely new type of mathematical element (e.g., matrix, limit, integral):

1. **Define the data structure**:
```javascript
let currentMatrix = null;
```

2. **Create enter/exit functions**:
```javascript
function enterMatrixMode() {
    currentMatrix = { rows: [[]], cols: 2 };
    expressionData.push({ type: 'matrix', data: currentMatrix });
    currentMode = 'matrix';
    render();
}

function exitMatrixMode() {
    currentMatrix = null;
    currentMode = 'normal';
}
```

3. **Handle input in insertCharacter**:
```javascript
else if (currentMode === 'matrix') {
    // Custom logic for matrix input
}
```

4. **Add rendering logic**:
```javascript
else if (elem.type === 'matrix') {
    html += renderMatrix(elem.data);
}
```

5. **Update raw expression builder**:
```javascript
else if (elem.type === 'matrix') {
    raw += `matrix([${elem.data.rows}])`;
}
```

---

## Browser Compatibility

### Supported Browsers

✅ **Chrome** 90+
✅ **Firefox** 88+
✅ **Safari** 14+
✅ **Edge** 90+
✅ **Opera** 76+

### Required Features

- **ContentEditable**: For the editable math display
- **beforeinput Event**: For intercepting keystrokes
- **SVG**: For rendering square root radicals
- **Flexbox**: For layout
- **CSS Grid**: For button layout
- **ES6 JavaScript**: Arrow functions, template literals, const/let

### Known Limitations

⚠️ **Mobile Browsers**: Virtual keyboards may interfere with special key handling (Arrow keys, Escape)

⚠️ **Internet Explorer**: Not supported (no ES6 support, no beforeinput event)

⚠️ **Older Browsers**: May not support all CSS features (CSS Grid, modern gradients)

### Polyfills

If you need to support older browsers, consider adding:

- [Babel](https://babeljs.io/) for ES6 transpilation
- [CSS Grid Polyfill](https://github.com/FremyCompany/css-grid-polyfill)
- [beforeinput Polyfill](https://github.com/GoogleChromeLabs/pointer-event-polyfill)

---

## Performance Considerations

### Rendering Optimization

The current implementation re-renders the entire display on every keystroke. For very long expressions (>1000 characters), this could become slow. Potential optimizations:

1. **Virtual DOM**: Only update changed elements
2. **Debouncing**: Delay raw expression updates
3. **Lazy Rendering**: Render only visible portions for very long expressions

### Memory Management

The expressionData array grows indefinitely. For production use, consider:

1. **Expression Length Limit**: Prevent runaway memory usage
2. **Periodic Cleanup**: Remove empty text nodes
3. **Undo/Redo Stack**: Maintain fixed-size history

---

## Accessibility

### Current Accessibility Features

- ✅ Semantic HTML structure
- ✅ Keyboard navigation support
- ✅ High contrast color scheme
- ✅ Focus indicators

### Potential Improvements

To make this tool more accessible:

1. **ARIA Labels**: Add `aria-label` attributes to buttons and sections
2. **Screen Reader Support**: Add live regions for result announcements
3. **Keyboard Shortcuts**: Document all keyboard interactions
4. **Math Accessibility**: Consider integrating MathML for screen readers

Example ARIA enhancement:

```html
<div id="mathDisplay" 
     contenteditable="true" 
     role="textbox"
     aria-label="Mathematical expression input"
     aria-multiline="true">
</div>

<button class="evaluate-btn" 
        aria-label="Calculate and display result"
        onclick="evaluateExpression()">
    Calculate Result
</button>
```

---

## Troubleshooting

### Common Issues

**Issue 1: Cursor Jumps Around**

*Solution*: This can happen if the render() function is called too frequently. Check that you're not triggering renders outside of normal input flow.

**Issue 2: Keywords Not Triggering**

*Solution*: Keywords only trigger when not preceded by another letter. Typing "cosine" won't trigger "cos" mode. This is intentional to prevent false positives.

**Issue 3: Can't Exit Special Mode**

*Solution*: Press Space, Tab, Enter, or Arrow Right. If stuck, press Escape to force exit.

**Issue 4: Evaluation Errors**

*Solution*: Check the raw expression display. Math.js requires specific syntax. Common issues:
- Use `*` for multiplication (not implicit)
- Use `pi` or `π` for π (automatic conversion)
- Trig functions expect radians

**Issue 5: Square Root Radical Wrong Size**

*Solution*: The radical height is set dynamically. If it's wrong, check that the CSS for `.sqrt-content` hasn't been modified.

### Debug Mode

To enable debug logging, add this to `script.js`:

```javascript
const DEBUG = true;

function debugLog(message, data) {
    if (DEBUG) {
        console.log(`[Math Input] ${message}`, data);
    }
}

// Then add throughout code:
function insertCharacter(char) {
    debugLog('Inserting character', { char, mode: currentMode });
    // ... rest of function
}
```

---

## Future Enhancements

### Planned Features

1. **Matrices**: Support for matrix notation and operations
2. **Integrals & Derivatives**: Calculus notation
3. **Limit Notation**: lim subscripts and arrow notation
4. **Summation & Product**: Σ and Π with subscript/superscript bounds
5. **Piecewise Functions**: Curly brace notation
6. **Copy/Paste**: LaTeX import/export
7. **Undo/Redo**: Full history management
8. **Themes**: Dark mode, high contrast, custom colors
9. **Mobile Optimization**: Touch-friendly UI, gesture support
10. **Collaboration**: Real-time shared editing

### Contribution Ideas

Want to contribute? Here are some good starting points:

- **Easy**: Add more function buttons, improve error messages
- **Medium**: Implement undo/redo, add keyboard shortcuts panel
- **Hard**: Add matrix support, implement LaTeX export

---

## Contributing

We welcome contributions! Here's how to get started:

### Development Setup

1. Fork the repository
2. Clone your fork: `git clone https://github.com/yourusername/scientific-math-input.git`
3. Create a branch: `git checkout -b feature/your-feature-name`
4. Make your changes
5. Test thoroughly in multiple browsers
6. Commit: `git commit -am 'Add some feature'`
7. Push: `git push origin feature/your-feature-name`
8. Create a Pull Request

### Code Style Guidelines

- Use **2 spaces** for indentation
- Use **camelCase** for variable and function names
- Add **comments** for complex logic
- Keep functions **small and focused** (under 50 lines)
- Use **meaningful variable names**

### Testing Checklist

Before submitting a PR, ensure:

- [ ] All existing features still work
- [ ] New features work in Chrome, Firefox, and Safari
- [ ] Code follows style guidelines
- [ ] No console errors
- [ ] README updated if adding new features

---

## License

This project is licensed under the MIT License.

```
MIT License

Copyright (c) 2024 Scientific Math Input Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## Credits

### Technologies Used

- **Math.js**: Mathematical expression evaluation - https://mathjs.org/
- **Google Fonts**: Typography (JetBrains Mono, Crimson Pro)
- **Pure JavaScript**: No frameworks required!

### Inspiration

This project was inspired by:
- Mathematical notation editors like MathType and MathQuill
- Computational tools like Wolfram Alpha
- The need for a lightweight, framework-free math input solution

---

## Contact & Support

### Get Help

- **Issues**: Report bugs at https://github.com/yourusername/scientific-math-input/issues
- **Discussions**: Ask questions at https://github.com/yourusername/scientific-math-input/discussions
- **Email**: support@example.com

### Stay Updated

- **Star** the repository to get notifications
- **Watch** for new releases
- Follow the project on social media

---

## Changelog

### Version 1.0.0 (Initial Release)

**Features**:
- ✅ Basic mathematical expression input
- ✅ Auto-formatting for sqrt, fractions, superscripts, functions, abs
- ✅ 24-button mathematical symbol palette
- ✅ Real-time expression evaluation
- ✅ Smart keyboard navigation
- ✅ Visual and raw expression views
- ✅ Error handling and validation
- ✅ Responsive design

**Known Issues**:
- Mobile keyboard handling could be improved
- No undo/redo functionality yet
- Limited to 2D mathematical notation

---

## Appendix

### Mathematical Notation Reference

| Notation | Input Method | Example |
|----------|--------------|---------|
| Square Root | Type `sqrt` | √x |
| Exponent | Type `^` | x² |
| Fraction | Type `/` | ³⁄₄ |
| Absolute Value | Type `abs` | \|x\| |
| Sine | Type `sin` | sin(x) |
| Pi | Click π button or type π | π |
| Euler's Number | Click e button | e |

### Keyboard Shortcuts Reference

| Shortcut | Action |
|----------|--------|
| `^` | Enter superscript mode |
| `/` | Enter fraction mode |
| `Space` | Exit current mode |
| `Tab` | Exit current mode |
| `Enter` | Exit current mode |
| `Escape` | Cancel current mode |
| `→` (Arrow Right) | Exit mode / Navigate fraction |
| `←` (Arrow Left) | Navigate to fraction numerator |
| `Backspace` | Delete character / Exit empty mode |

### Math.js Function Reference

All standard Math.js functions are supported. Common ones:

**Trigonometry**: sin, cos, tan, asin, acos, atan, atan2, sinh, cosh, tanh

**Exponential & Logarithmic**: exp, log, log10, sqrt, cbrt, pow

**Rounding**: ceil, floor, round, fix

**Arithmetic**: abs, sign, gcd, lcm, mod

**Constants**: pi, e, i, tau, phi

For complete reference: https://mathjs.org/docs/reference/functions.html

---

**End of README**

Thank you for using Scientific Math Input! We hope this tool makes mathematical expression editing easier and more intuitive. Happy calculating! 🎯
