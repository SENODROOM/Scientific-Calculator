# Scientific Calculator Code Explanation

## Overview

This is a web-based scientific calculator built with HTML, CSS, and JavaScript. It features a modern dark-themed interface with mathematical notation rendering using MathJax, supporting both basic arithmetic and advanced scientific functions.

## Structure

### HTML Structure

The calculator consists of three main components:

1. **Mode Toggle** - Switches between degrees (DEG) and radians (RAD) for trigonometric functions
2. **Display Area** - Shows the current expression and results using LaTeX formatting
3. **Button Grid** - 5-column layout containing all calculator buttons

### CSS Styling

The design uses a dark color scheme with the following key features:

- **Background**: Deep blue-gray (`#0f172a`) for the page, nearly black (`#020617`) for the calculator
- **Layout**: Centered flexbox layout with rounded corners and shadow effects
- **Grid System**: 5-column responsive grid with 8px gaps between buttons
- **Color Coding**: 
  - Green for equals button (`#22c55e`)
  - Red for clear button (`#ef4444`)
  - Dark gray for standard buttons (`#1e293b`)

### JavaScript Functionality

## Core Variables

```javascript
let expr = "";      // Stores the current expression
let degree = true;  // Toggle between degrees/radians mode
```

## Key Functions

### 1. Mode Management

**`toggleMode()`**
- Switches between degree and radian modes
- Updates the mode button text accordingly

### 2. Expression Building

**`add(x)`**
- Appends input to the current expression
- Triggers display rendering

**`back()`**
- Removes the last character from the expression
- Implements backspace functionality

**`clearAll()`**
- Resets the expression to empty
- Clears the display

### 3. Display Rendering

**`toLatex(s)`**
- Converts the calculator expression to LaTeX format for beautiful mathematical notation
- Transformations include:
  - `pi` → `\pi` (π symbol)
  - `sqrt(x)` → `\sqrt{x}` (square root notation)
  - `^n` → `^{n}` (exponents)
  - `a/b` → `\frac{a}{b}` (fraction notation)

**`render()`**
- Updates the display with LaTeX-formatted expression
- Calls MathJax to typeset the mathematical notation

### 4. Calculation Engine

**`toJS(s)`**
- Converts calculator syntax to JavaScript-executable code
- Handles mathematical functions:
  - `pi` → `Math.PI`
  - `^` → `**` (exponentiation operator)
  - `sqrt` → `Math.sqrt`
  - `log` → `Math.log10` (base-10 logarithm)
  - `ln` → `Math.log` (natural logarithm)
  
- **Trigonometric functions**: Converts based on mode
  - **Degree mode**: Wraps arguments with `Math.PI/180*` for conversion
  - **Radian mode**: Direct mapping to `Math.sin`, `Math.cos`, `Math.tan`

**`calc()`**
- Evaluates the expression using JavaScript's Function constructor
- Validates results (checks for infinity/NaN)
- Updates expression with result or shows error message
- Error handling catches syntax errors and invalid operations

### 5. Keyboard Support

The calculator supports keyboard input:
- **Number keys (0-9)**: Enter digits
- **Operators (+, -, *, /)**: Perform operations
- **Parentheses and decimal**: Direct input
- **Enter**: Calculate result
- **Backspace**: Delete last character
- **Escape**: Clear all

## Supported Functions

### Basic Operations
- Addition (+), Subtraction (−), Multiplication (×), Division (÷)
- Decimal numbers
- Parentheses for grouping

### Scientific Functions
- **Trigonometry**: sin, cos, tan (with DEG/RAD mode)
- **Logarithms**: log (base-10), ln (natural log)
- **Powers**: x², x³, x^n (custom exponent)
- **Square root**: √x
- **Constants**: π (pi)

## Mathematical Notation

The calculator displays expressions in proper mathematical notation thanks to MathJax:
- Fractions appear as stacked numerator/denominator
- Square roots show the radical symbol
- Exponents are superscripted
- Pi displays as the Greek letter π

## Example Usage Flow

1. User clicks buttons or types on keyboard
2. Expression builds up (e.g., "sin(30)+sqrt(16)")
3. Display renders in LaTeX: sin(30) + √16
4. User presses "=" or Enter
5. Expression converts to JavaScript: `Math.sin(Math.PI/180*30)+Math.sqrt(16)`
6. Result calculates: approximately 4.5
7. Display updates with the numerical result

## Error Handling

The calculator includes robust error handling:
- Catches syntax errors in expressions
- Detects infinite or undefined results
- Displays "Error" message in LaTeX format
- Automatically clears invalid expressions

## Design Patterns

- **Separation of concerns**: Display logic separate from calculation logic
- **Dual representation**: Human-readable LaTeX for display, JavaScript-compatible for computation
- **Mode-aware calculation**: Trigonometric functions adjust based on angle unit preference
- **Progressive enhancement**: Keyboard support enhances button interface

## Browser Dependencies

- **MathJax 3**: Loaded from CDN for mathematical typesetting
- **Modern JavaScript**: Uses ES6+ features (arrow functions, template literals)
- **CSS Grid**: For responsive button layout