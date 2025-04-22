# Scientific Calculator Implementation for WordPress

Based on my analysis of the DevCalc Scientific Calculator, I'll provide you with a custom implementation approach that you can integrate into your WordPress site.

## Implementation Overview

Since we couldn't extract the exact code from the original site (as it appears to be using a JavaScript framework with bundled code), we'll create our own implementation that matches the functionality and appearance of the original calculator.

## Implementation Steps

### 1. Create a Custom WordPress Page

First, create a new page in WordPress where you'll host the calculator:

1. Go to your WordPress admin dashboard
2. Navigate to Pages > Add New
3. Give it a title like "Scientific Calculator"
4. Switch to the "Custom HTML" block or "Code Editor" view

### 2. HTML Structure

Add this HTML structure to your page:

```html
<div class="calculator-container">
  <div class="calculator-wrapper">
    <div class="calculator">
      <div class="display">
        <div id="display-value">0</div>
      </div>
      
      <div class="calculator-buttons">
        <!-- Row 1 -->
        <button class="btn function-btn" id="clear">AC</button>
        <button class="btn function-btn" id="delete">DEL</button>
        <button class="btn trig-btn" id="sin">sin</button>
        <button class="btn trig-btn" id="cos">cos</button>
        <button class="btn trig-btn" id="tan">tan</button>
        <button class="btn operator-btn" id="divide">÷</button>
        
        <!-- Row 2 -->
        <button class="btn constant-btn" id="pi">π</button>
        <button class="btn constant-btn" id="e">e</button>
        <button class="btn function-btn" id="log">log</button>
        <button class="btn function-btn" id="ln">ln</button>
        <button class="btn operator-btn" id="multiply">×</button>
        
        <!-- Row 3 -->
        <button class="btn number-btn" id="seven">7</button>
        <button class="btn number-btn" id="eight">8</button>
        <button class="btn number-btn" id="nine">9</button>
        <button class="btn function-btn" id="square">x²</button>
        <button class="btn operator-btn" id="subtract">-</button>
        
        <!-- Row 4 -->
        <button class="btn number-btn" id="four">4</button>
        <button class="btn number-btn" id="five">5</button>
        <button class="btn number-btn" id="six">6</button>
        <button class="btn function-btn" id="sqrt">√</button>
        <button class="btn operator-btn" id="add">+</button>
        
        <!-- Row 5 -->
        <button class="btn number-btn" id="one">1</button>
        <button class="btn number-btn" id="two">2</button>
        <button class="btn number-btn" id="three">3</button>
        <button class="btn function-btn" id="power">x^y</button>
        <button class="btn operator-btn" id="equals">=</button>
        
        <!-- Row 6 -->
        <button class="btn number-btn" id="zero">0</button>
        <button class="btn number-btn" id="decimal">.</button>
        <button class="btn parenthesis-btn" id="left-paren">(</button>
        <button class="btn parenthesis-btn" id="right-paren">)</button>
      </div>
    </div>
  </div>
</div>
```

### 3. CSS Styling

Add this CSS to match the appearance of the original calculator:

```css
<style>
  .calculator-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    font-family: Arial, sans-serif;
  }
  
  .calculator-wrapper {
    background-color: #f5f5f5;
    border-radius: 10px;
    padding: 20px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  .calculator {
    display: flex;
    flex-direction: column;
    gap: 15px;
  }
  
  .display {
    background-color: #2c3e50;
    color: white;
    padding: 20px;
    border-radius: 5px;
    text-align: right;
    font-size: 2.5rem;
    min-height: 80px;
    display: flex;
    align-items: center;
    justify-content: flex-end;
  }
  
  .calculator-buttons {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 10px;
  }
  
  .btn {
    padding: 15px 10px;
    border: none;
    border-radius: 5px;
    font-size: 1.2rem;
    cursor: pointer;
    transition: background-color 0.2s;
  }
  
  .function-btn {
    background-color: #e8f4f8;
  }
  
  .operator-btn {
    background-color: #4a90e2;
    color: white;
  }
  
  .number-btn {
    background-color: #f9f9f9;
  }
  
  .trig-btn, .constant-btn {
    background-color: #f0f0f0;
  }
  
  .parenthesis-btn {
    background-color: #f0f0f0;
  }
  
  .btn:hover {
    opacity: 0.8;
  }
  
  #clear, #delete {
    background-color: #e74c3c;
    color: white;
  }
  
  #equals {
    background-color: #2ecc71;
    color: white;
  }
  
  /* Responsive design */
  @media (max-width: 600px) {
    .calculator-buttons {
      grid-template-columns: repeat(4, 1fr);
    }
    
    .btn {
      padding: 12px 8px;
      font-size: 1rem;
    }
    
    .display {
      font-size: 2rem;
      min-height: 60px;
    }
  }
</style>
```

### 4. JavaScript Functionality

Add this JavaScript to implement the calculator functionality:

```javascript
<script>
document.addEventListener('DOMContentLoaded', function() {
  // Calculator state
  let displayValue = '0';
  let firstOperand = null;
  let waitingForSecondOperand = false;
  let operator = null;
  let memory = 0;
  
  // DOM elements
  const display = document.getElementById('display-value');
  const buttons = document.querySelectorAll('.btn');
  
  // Update display
  function updateDisplay() {
    display.textContent = displayValue;
  }
  
  // Clear calculator
  function clearCalculator() {
    displayValue = '0';
    firstOperand = null;
    waitingForSecondOperand = false;
    operator = null;
    updateDisplay();
  }
  
  // Delete last character
  function deleteLastChar() {
    if (displayValue.length > 1) {
      displayValue = displayValue.slice(0, -1);
    } else {
      displayValue = '0';
    }
    updateDisplay();
  }
  
  // Handle number input
  function inputDigit(digit) {
    if (waitingForSecondOperand) {
      displayValue = digit;
      waitingForSecondOperand = false;
    } else {
      displayValue = displayValue === '0' ? digit : displayValue + digit;
    }
    updateDisplay();
  }
  
  // Handle decimal point
  function inputDecimal() {
    if (waitingForSecondOperand) {
      displayValue = '0.';
      waitingForSecondOperand = false;
      updateDisplay();
      return;
    }
    
    if (!displayValue.includes('.')) {
      displayValue += '.';
      updateDisplay();
    }
  }
  
  // Handle operators
  function handleOperator(nextOperator) {
    const inputValue = parseFloat(displayValue);
    
    if (operator && waitingForSecondOperand) {
      operator = nextOperator;
      return;
    }
    
    if (firstOperand === null) {
      firstOperand = inputValue;
    } else if (operator) {
      const result = performCalculation();
      displayValue = String(result);
      firstOperand = result;
    }
    
    waitingForSecondOperand = true;
    operator = nextOperator;
    updateDisplay();
  }
  
  // Perform calculation
  function performCalculation() {
    const secondOperand = parseFloat(displayValue);
    
    if (operator === '+') {
      return firstOperand + secondOperand;
    } else if (operator === '-') {
      return firstOperand - secondOperand;
    } else if (operator === '×') {
      return firstOperand * secondOperand;
    } else if (operator === '÷') {
      return firstOperand / secondOperand;
    } else if (operator === 'x^y') {
      return Math.pow(firstOperand, secondOperand);
    }
    
    return secondOperand;
  }
  
  // Handle scientific functions
  function handleScientificFunction(func) {
    const inputValue = parseFloat(displayValue);
    let result;
    
    switch(func) {
      case 'sin':
        result = Math.sin(inputValue);
        break;
      case 'cos':
        result = Math.cos(inputValue);
        break;
      case 'tan':
        result = Math.tan(inputValue);
        break;
      case 'log':
        result = Math.log10(inputValue);
        break;
      case 'ln':
        result = Math.log(inputValue);
        break;
      case 'x²':
        result = Math.pow(inputValue, 2);
        break;
      case '√':
        result = Math.sqrt(inputValue);
        break;
      default:
        return;
    }
    
    displayValue = String(result);
    firstOperand = result;
    waitingForSecondOperand = true;
    updateDisplay();
  }
  
  // Handle constants
  function handleConstant(constant) {
    if (constant === 'π') {
      displayValue = String(Math.PI);
    } else if (constant === 'e') {
      displayValue = String(Math.E);
    }
    updateDisplay();
  }
  
  // Handle parentheses
  function handleParenthesis(paren) {
    if (displayValue === '0' || waitingForSecondOperand) {
      displayValue = paren;
      waitingForSecondOperand = false;
    } else {
      displayValue += paren;
    }
    updateDisplay();
  }
  
  // Event listeners for buttons
  buttons.forEach(button => {
    button.addEventListener('click', () => {
      // Numbers
      if (button.classList.contains('number-btn')) {
        inputDigit(button.textContent);
      }
      
      // Decimal
      if (button.id === 'decimal') {
        inputDecimal();
      }
      
      // Operators
      if (button.id === 'add') {
        handleOperator('+');
      } else if (button.id === 'subtract') {
        handleOperator('-');
      } else if (button.id === 'multiply') {
        handleOperator('×');
      } else if (button.id === 'divide') {
        handleOperator('÷');
      } else if (button.id === 'power') {
        handleOperator('x^y');
      }
      
      // Equals
      if (button.id === 'equals') {
        if (!operator) return;
        
        displayValue = String(performCalculation());
        operator = null;
        firstOperand = null;
        waitingForSecondOperand = false;
        updateDisplay();
      }
      
      // Clear and Delete
      if (button.id === 'clear') {
        clearCalculator();
      } else if (button.id === 'delete') {
        deleteLastChar();
      }
      
      // Scientific functions
      if (button.classList.contains('trig-btn') || 
          button.id === 'log' || 
          button.id === 'ln' || 
          button.id === 'square' || 
          button.id === 'sqrt') {
        handleScientificFunction(button.textContent);
      }
      
      // Constants
      if (button.classList.contains('constant-btn')) {
        handleConstant(button.textContent);
      }
      
      // Parentheses
      if (button.classList.contains('parenthesis-btn')) {
        handleParenthesis(button.textContent);
      }
    });
  });
  
  // Initialize display
  updateDisplay();
});
</script>
```

### 5. WordPress Integration Options

There are several ways to integrate this code into your WordPress site:

#### Option A: Custom HTML Block
1. Create a new page in WordPress
2. Add a Custom HTML block
3. Paste the HTML, CSS, and JavaScript code into the block
4. Publish the page

#### Option B: Custom Shortcode
1. Add this code to your theme's functions.php file or a custom plugin:

```php
function scientific_calculator_shortcode() {
    ob_start();
    ?>
    <!-- Paste the HTML, CSS, and JavaScript code here -->
    <?php
    return ob_get_clean();
}
add_shortcode('scientific_calculator', 'scientific_calculator_shortcode');
```

2. Use the shortcode `[scientific_calculator]` on any page or post

#### Option C: Custom Block Plugin
For a more professional integration, you could create a custom Gutenberg block plugin:

1. Create a new plugin folder in wp-content/plugins/
2. Create the necessary plugin files
3. Register a custom block that includes the calculator code
4. Activate the plugin in WordPress

## Additional Considerations

### Mobile Responsiveness
The CSS includes media queries for mobile responsiveness, but you may need to adjust them based on your WordPress theme.

### Theme Compatibility
The calculator styling may need adjustments to match your WordPress theme's design.

### Performance
Consider loading the JavaScript code only on pages where the calculator is used to improve site performance.

### Browser Compatibility
The code uses modern JavaScript features that work in all current browsers but may need polyfills for older browsers.

## Next Steps

1. Choose your preferred integration method
2. Implement the code on your WordPress site
3. Test the calculator functionality thoroughly
4. Make any necessary adjustments to match your site's design
