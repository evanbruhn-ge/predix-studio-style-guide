![Predix Studio logo](https://www.predix.io/assets/images/landing/studio_logo_2x.png)

# Style Guide (WORK IN PROGRESS)
###### For JavaScript and SCSS

Targeting **Predix Studio** and **Predix App Engine** development. This references a subset of AirBnB's ES5 and ES6 style guides, tailored to suit Studio and App Engine's front-end stack and development patterns. SCSS style guide references a subset of AirBnB's and codeguide.co's SCSS and CSS style guides in addition to our guidelines.

## Table of Contents
1. [JavaScript Style Guide](#javascript-style-guide)
	1. [Introduction](#javascript-intro)
		1. [Linting](#javascript-linting)
		2. [Enforcement](#javascript-enforcement)
	2. [Glossary](#javascript-glossary)
	3. [Functional Values](#javascript-functional-values)
	4. [Architecture](#javascript-architecture")
	5. [Re-usability](#javascript-reuse") 
	6. [Whitespace](#javascript-whitespace)
	7. [Naming Conventions](#javascript-naming)
	8. [Functions](#javascript-functions)
	9. [Composable Functions](#javascript-composable-functionss)
	10. [state](#javascript-state)
	11. [Variables and References](#javascript-variables)
	12. [Constants](#javascript-constants)
	13. [Configurations](#javascript-config)
	14. [New](#javascript-new)
	15. [Quotes](#javascript-quotes)
	16. [Comments](#javascript-comments)
	17. [Comparison Operators & Equality](#javascript-operators)
	18. [jQuery](#javascript-jquery)
	19. [Loops](#javascript-loops)
	20. [Partial Evaluation](#javascript-partial-evaluation)
2. [SCSS Style Guide](#scss-style-guide)
	1. [Linting](#scss-linting)
	2. [Glossary](#scss-glossary)
	3. [Whitespace](#scss-whitespace)
	4. [Naming Conventions](#scss-naming)
	5. [Units](#scss-units)
	6. [Colors](#scss-colors)
	7. [Selectors](#scss-selectors)
3. [Contributing to this style guide](#contributing)
	   	

## <a name="javascript-style-guide"></a>Javascript Style Guide
* **<a name="javascript-intro"></a>1.1 Introduction**

  Hello! If this is your first time reading this style guide, we encourage you to read this section in full. Otherwise feel free to [skip directly to the meat of this style guide](#javascript-whitespace).

  Thanks for your time! 

* **<a name="javascript-linting"></a>1.1.1 Linting**

  This repo includes javascript linting configurations for all mainstream IDEs, including: IntelliJ, Sublime Text, Atom, VS Code and Eclipse. Any IDE which supports ESlint should also work.

  Unless otherwise stated, all guidelines in this section are linted at the **ERROR** level. All code committed must comply with these guidelines otherwise automated tests may fail and/or your pull request may be rejected. If there's a reasonable reason why a guideline could not be followed, please note this in your pull request and pass your feedback to the UX team.

  A number of other best practices from the AirBnB style guide are implemented in the linting configurations at the **WARNING** level. You are encouraged but not required to follow these guidelines.

* **<a name="javascript-enforcement"></a>1.1.2 Enforcement**

  In general, please adhere to all guidelines below and/or any linting exceptions at the **ERROR** level.

  Automated static code analysis for BitBucket is being considered. Integration of the bundled ESLint configuration in Studio automated test suites is planned but not yet implemented.

  Developers are encouraged to call out style guide issues in other PRs as part of code review. 

* **<a name="javascript-glossary"></a>1.2 Glossary**

  Term/Symbol | Definition
  ------------- | -------------
  ...  | Represents written code

* **<a name="javascript-functional-values"></a>1.3 Functional Values**

* These are the values we uphold with this guide and hope that this will help at releasing better software more regularly and steadily. There are a number of benefits with functional programming, namely more robust, clearer code that is easier to test, and review and maintain. You no longer have to imagine what consequences of mutating this field for that components will be. You just don’t have to think broader than the ten lines of code you’re looking at right now.

   On the downside many articles about functional programming are written by nerdy mathematician point scoring types. Reading them without preliminary training is dangerous: categories and morphisms can blow your mind in exchange for nothing.

   “Writing pure functions is easy, but combining them into a complete application is where things get hard”

   Our aim here is too adopt some of the more sane elements of the functional approach that will deliver the majority of the benefits without getting lost in the esoteric maths of category theory. The focus is not about lambda calculus, monads, morphisms, and combinators. It’s about having many small well-defined composable functions without mutations of global state, their arguments, and IO.”

   “In other words, if point-free style helps to communicate better in a particular case, use it. Otherwise, don’t.”

   Switching to small composable functions without side effects where possible will give us most of the benefits of functional programming without the headache inducing extremes of the discipline.

   This guide is written with the

   * Functional is the best way
   * Impurity must be isolated, mark it as impure, either by design or pragmatism.
   * Composable code is encouraged

* **<a name="javascript-architecture"></a>1.4 Architecture**

   * A well architectured' javascript program is done through the module architectural pattern. Which decouple modules (components) and expose composable interfaces. Therefore it is paramount to properly apply the module pattern.

    So what is a module? It is a self-contained program, that takes an input and return an output. This program can be pure or impure. But a pure program cannot receive an impure input. So expect inputs to be data. This program can have any number of private functions to achieve its goal. But as a general idea, it should have only one interface -- one export.

    A module can require or import as many modules it needs to achieve its goal. But you should try to keep the module small and centered around one responsibility.

    A module should be in its own file, and all of its sub-modules in the same directory. In this manner, a composed module can be replaced or deleted from the javascript program without any major side effects.

    One could say that a complex javascript program is a collection of modules that could be replaced with a module with the same public interface.

* **<a name="javascript-reuse"></a>1.5 Re-usability**

  * Opposite to what some people may think, the idea of reusability is not so much a goal but a side effect in functional programming. It's something you attain because you design the components properly, not because you avoid copying every piece of code. We prefer to see code that will never change because it has only one responsibility, be copied over as a private function into another module rather than the same code as part of a utility or its own module. Moreover, think like this, copy the code first, use unit tests to document the functionality, globalization of the code is the last resort thing.

* **<a name="javascript-whitespace"></a>1.6 Whitespace**

  * Use soft tabs set to 2 spaces. eslint: [`indent`](http://eslint.org/docs/rules/indent.html) jscs: [`validateIndentation`](http://jscs.info/rule/validateIndentation)

    > Why? There's no objectively "best" way to do indentation, but consistency keeps us all sane. 2 space soft tabs also aligns with other Predix development teams and guarantees code will render the same in all environments.

    ```javascript
    // bad
    function foo() {
    ∙∙∙∙const name;
    }

    // bad
    function bar() {
    ∙const name;
    }

    // good
    function baz() {
    ∙∙const name;
    }
    ```

  * Set off operators with spaces. eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops.html) jscs: [`requireSpaceBeforeBinaryOperators`](http://jscs.info/rule/requireSpaceBeforeBinaryOperators), [`requireSpaceAfterBinaryOperators`](http://jscs.info/rule/requireSpaceAfterBinaryOperators)

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  * Write if, when and for statements with a new line separating the test, the executed code and the closing brace, unless your code is reasonably short.
  
    > Not automatically enforced or linted. Use your best judgement, favouring whichever is the most readable.

    ```javascript
    // bad
    if (longAndComplicatedTest) { lots(); of(); code(); ... }

    // bad
    if (longAndComlicatedTest) { lots();
      of();
      code();
    .. }

    // ok
    if (simpleTest) doThing();

    // good
    if (...) {
      ...
    }	
      ```

  * Do not add spaces inside brackets, just follow english grammar rules. eslint: [`array-bracket-spacing`](http://eslint.org/docs/rules/array-bracket-spacing.html) jscs: [`disallowSpacesInsideArrayBrackets`](http://jscs.info/rule/disallowSpacesInsideArrayBrackets)

    ```javascript
    // bad
    const foo = [ 1, 2, 3 ];

    // bad
    const foo = [1,2,3];

    // still bad
    const foo = [item1,item2,item3]

    // good
    const foo = [1, 2, 3];
    ```

* **<a name="javascript-naming"></a>1.7 Naming Conventions**

  * Avoid overuse of the helper naming. A function has only one purpose and its name should represent that. Moreover, it should be private to its module. Although it may cause code duplication, it will decrease complexity by decoupling modules.
  * Use camelCase when naming objects, functions, and instances. eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html) jscs: [`requireCamelCaseOrUpperCaseIdentifiers`](http://jscs.info/rule/requireCamelCaseOrUpperCaseIdentifiers)

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  * Use PascalCase only when naming constructors or classes. eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html) jscs: [`requireCapitalizedConstructors`](http://jscs.info/rule/requireCapitalizedConstructors)
  
    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```
* **<a name="javascript-functions"></a>1.8 Functions**

  * Avoid function declaration and use function expression.

    ```javascript
    // bad
    function foo() { ... }

    // good
    const foo = () => ...

    // generators
    // bad
    function* foo() { ... }

    // good
    const foo = function* () { ... }

    // async functions
    // bad
    async function foo () { ... }

    // good
    const foo = async () => { ... }
    ```

  * Function declarations should be used for module-level functions, arrow functions should be used everywhere else.

    ```javascript

    // Document level
    const foo = function() { ... }
    
        // Within a block
        const foo = () => ...
        function getBigSalad(){
        return salads.filter(salad => salad.size === 'big')[0]
    }
    
    function changeButtonState(button) {
        return DMWComponent.writeButtonState(button.id, button.state);
    }

    function getAnimalButtons() {
        return DMWComponent.animalButtons;
    }

    function alwaysBigWoof(woofSounds) {
        return woofSounds.find(sound => {
          sound.type === 'bigwoof';
        });
    }

    async function animalNoises() {
         await alwaysBigWoof();
    }
    ```

   * One Input, One Output encouraged

    ```javascript
    // bad
    const add = (a, b) => a + b
    
    const foo = (a, b, c, d, e) => (/* ... */)
    const foo = a => b => c => d => e => (/* ... */)
    
    // ok
    const add = ({a, b}) => a + b
    
    // best
    const add = a => b => a + b

    which can be called as follows:
    add(1)(2); // 3
    ```

    * Single Returns encouraged

    ```javascript
        // bad
        const foo = a => {
        if (!a){
            return bar(0)
        }
        return bar(a)
        }
        
        // ok
        const foo = a => bar(a ? a : 0)
        
        // best
        const foo (a = 0) =>  bar(a)
    ```

    * Document Impurity

    Impure functions or impure function groups must be commented with @sideeffect ( ideally needs to be made pure eventually) or @intendedsideeffects if it is impure by design. Pure functions can be marked  @nosideeffects for clarity.

    ```javascript
        /**
	* @nosideeffects
	*/
        const upper = a => s.toUpperCase()
        const selectBody = res => res.body
        
        /**
	* @intendedsideeffects
	*/
        const requestBodyToUpperCase = compose(upper, selectBody, getHttp)

        /**
	* @sideeffect
	*/
        const requestBodyToUpperCase = compose(upper, selectBody, getHttp)
    ```

* **<a name="javascript-composable-functionss"></a>1.9 Composable Functions**

  * Split code into composable functions.
  
    ```javascript
    // bad
    const splitToKeyValuePair = headerString => {
    return headerString.split(',')
        .reduce((result, current) => {
        const keyValuePair = current.split('=')
        const key = keyValuePair[0]
        const value = keyValuePair[1]
        result[key] = value
        return result
        }, {})
    }
    
    // good
    const splitToKeyValuePair = compose(combine, fromPairs, map(split('=')), map(trim), split(','))
    ```

* **<a name="javascript-state"></a>1.10 State**

  * Assignments & State Modification.
    As a general rule you should avoid assignments, they alter state and increase the risk of sharing state in code and changing functions parameters.

    ```javascript
    // bad
    const foo = state => {
    state.count = state.count + 1
    return state
    }
    
    // good
    const foo = state => object.assign({}, state, { count: state.count + 1 })
    
    // best
    const foo = state => ({ ...state, count: state.count + 1 })
    ```

  * Do not share state

    ```javascript
    // bad
    const inc = () => state.count++
    
    // good
    const inc = (state = { count: 0 }) => ({ count : state.count + 1 })
    ```  

* **<a name="javascript-variables"></a>1.11 Variables and References**

  * var is verboten

  * Always use `const` or `let` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  * If you must reassign references, use `let` instead of `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

    > Why? `let` is block-scoped rather than function-scoped like `var`.

    ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  * Keep variables closer to usage, inside function block if possible and just prefer using string literals.

    ```javascript
    // bad
    const INC = 2
    ...
    const increment = a => a + INC
    
    // OK
    const increment = a => {
    const INC = 2
    return a + INC
    }
    
    // good
    const incrementBy2 = a =>  a + 2
    ```

* **<a name="javascript-constants"></a>1.12 Constants**

  * Use string, boolean or number literals. But if you use constants, they must be close to usage, part of the module that uses them.

      ```javascript
    // good
    const userCountReducer = (userCount = 5) => ...
    
    // good
    const USER_DEFAULT_COUNT = 5
    const userCountReducer = (state = USER_DEFAULT_COUNT) => ...
    
    // good
    const USER_DEFAULT_COUNT = 5
    const fromDefaults = ()=>  ({ userCount: USER_DEFAULT_COUNT })
    
    // good
    const fromDefaults = () => ({ userCount: 5 })
    ```

* **<a name="javascript-config"></a>1.13 Configurations**
  * Constants are often used to implement configuration - parameters to the program as a whole that need to be changeable independently of code. In these cases, globality is more reasonable; however you should still try to think of ways to make configuration specific to the module you're working on.

  ```javascript
    // good
    const { USER_DEFAULT_COUNT } = require('./configuration')
    ... // a few lines doen but not too far
    const userCountReducer = (state = USER_DEFAULT_COUNT) => ...
    // bad
    const USER_DEFAULT_COUNT = 5
    ...
    const reducer = (state = USER_DEFAULT_COUNT) => ... // you just can't know that the default is 5, it makes no sense
    
    // good
    const reducer = (state = 5) => ...
  ```

* **<a name="javascript-new"></a>1.14 New**

  * Use with caution to avoid side effects

* **<a name="javascript-quotes"></a>1.15 Quotes**

  * Use single quotes `''` for strings. eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)

    ```javascript
    // bad
    const name = "This is not as cool, but still a string";

    // good
    const name = 'This is an awesome string of many characters';
    ```
    
* **<a name="javascript-comments"></a>1.16 Comments**

  * Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it’s on the first line of a block.

    ```javascript
    // bad
    const active = true;  // is current tab

    // bad 

    /* is current tab */
    const active = true;

    // good

    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }
    ```

  * Use `/** ... */` for multi-line comments. eslint: [`multiline-comment-style`](https://eslint.org/docs/rules/multiline-comment-style) 

    ```javascript
    // bad

    // What is love?
    // Baby don't hurt me
    // Don't hurt me
    // No more

    // good

    /**
      * What is love?
      * Baby don't hurt me
      * Don't hurt me
      * No more
    */
    ```
    
  * Use JSDoc style multi-line comments when writing functions. eslint: [`require-jsdoc`](https://eslint.org/docs/rules/require-jsdoc).

    > Linted at the warning level for now, you are not required to add this when maintaining existing code but we'd grateful if you did!
  
    ```javascript
    // bad, no documentation
    function secretSauce(magicNum1, magicNum2) {
      ...
    }

    // bad, wrong style

    // Adds two numbers together.
    function secretSauce(magicNum1, magicNum2) {
      ...
    }

    // bad, right content but doesn't follow multi-line comment style.

    // Adds two numbers together.
    // @param {int} magicNum1 The first number 
    // @param {int} magicNum2 The first number
    // @returns {int} The sum of the two numbers 
    function secretSauce(magicNum1, magicNum2) {
      ...
    }

    // good
    /**
      * Adds two numbers together.
      * @param {int} magicNum1 The first number.
      * @param {int} magicNum2 The second number.
      * @returns {int} The sum of the two numbers.
    */
    function secretSauce(magic1, magicNum2) {
      ...
    } 
    ```    
   
* **<a name="javascript-operators"></a>1.17 Comparison Operators & Equality**

  * Use `===` and `!==` over `==` and `!=`. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

    ```javascript
    // bad
    const a = 1
    a == 1 (returns true)
    a == '1' (returns true)
    a == true (returns true)

    // good
    const b = 1
    b === 1 (returns true)
    b === '1' (returns false)
    b === true (returns false)
    ```

  * Conditional statements such as the `if` statement evaluate their expression using coercion with the `ToBoolean` abstract method and always follow these simple rules:

    - **Objects** evaluate to **true**
    - **Undefined** evaluates to **false**
    - **Null** evaluates to **false**
    - **Booleans** evaluate to **the value of the boolean**
    - **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    - **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0] && []) {
      // true
      // an array (even an empty one) is an object, objects will evaluate to true
    }
    ```
  * Use shortcuts for booleans, but explicit comparisons for strings and numbers.

    ```javascript
    // bad
    if (isValid === true) {
      // ...
    }

    // good
    if (isValid) {
      // ...
    }

    // bad
    if (name) {
      // ...
    }

    // good
    if (name !== '') {
      // ...
    }

    // bad
    if (collection.length) {
      // ...
    }

    // good
    if (collection.length > 0) {
      // ...
    }
    ```
   
* **<a name="javascript-jquery"></a>1.18 jQuery**

  * jQuery is not available for use in the v11 API and must not be added without discussion with the UX team.
  * jQuery is still available for use in the v10 API, refer to (some link to an old revision of the guide) for guidelines on its use"

* **<a name="javascript-loops"></a>1.19 Loops**

    * For, Foreach but not while.
    Loops are inherently imperative. They also mix concerns: iteration and execution are two different concerns that, if handled separately from each other, result in more flexible code.

    Better options include:

    recursion

    map

    filter

    reduce

    * While is considered redundant.
    * Recursion, use Tail Call
    
    Tail call recursion is safer and allows for optimization.

    To make a tail call recursion you need to place the function call at the end of your function and have it return the value. see es6-recursion-tail-recursion

    ```javascript
    // recur :: Number -> Number -> Number
    const recur = n => acc =>  n == 0 ? acc : recur(n-1)(n * acc)
    
    // recur :: Number -> Number
    const factorial = (n) => recur(n)(1)
    ```

* **<a name="javascript-partial-evaluation"></a>1.20 Partial Evaluation**
   * Partially applied functions can prove useful for functions that are re-used, but are not a requirement, more details can be found here.

## <a name="scss-style-guide"></a>SCSS style guide

* **<a name="scss-linting"></a>2.1 Linting**

  Any IDE which implements an interface to scss-lint is supported. With plugins, all mainstream IDEs are supported, including but not limited to: IntellIJ, Sublime Text, Atom, VS Code and Eclipse.

  If given the choice, favour SCSS lint plugins which use the Dart implementation of Sass. The ruby implementation of Sass is deprecated and no longer maintained.

* **<a name="scss-glossary"></a>2.2 Glossary**

  Symbol | Definition
  ------------- | -------------
  ...  | Represents written code
  Rule declaration | A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties.
  Selectors | In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. For example, `.my-class` is a selector which targets all elements with the `my-class` class.
  Properties | Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. For example, `width: 1.5rem`.

* **<a name="scss-whitespace"></a>2.3 Whitespace**

  * Rule declarations must always be on new lines. 

    ```css
    // Bad
    .my-bad-class { width:2rem; height: 4rem; margin: 2rem 1rem; }
    }

    // Good
    .my-good-class {
      width: 2rem;
      height: 4rem;
      margin: 2rem 1rem;
    }
    ```

  * Use soft tabs set to 2 spaces for indentation. scss-lint: [`Indentation`](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#indentation)

    ```css
    // Bad
    .my-class-is-too-cold {
    ∙width:2rem;
    }

    // Still bad
    .my-class-is-too-hot {
    ∙∙∙∙width:2rem;
    }

    // Good
    .my-class-is-juuust-right {
    ∙∙width: 2rem;  
    }	
    ```

* **<a name="scss-naming"></a>2.4 Naming convention**

  * Prefer dashes over camelCasing when naming classes, SCSS variables, functions and mix-ins. scss-lint: [`NameFormat`](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#nameformat)

    > Why? For class names, they serve as natural breaks in related classes (eg. `.btn and .btn-danger`) and support the `|=` selector specifier (eg. `[class|="btn"]` will target all classes with the `btn` token). In general, it's also aids in readability and is clearly differentiated from javascript's naming convention.

    ```css
    // Bad
    .myCoolWidget {
      ...
    }

    // Bad
    $myCoolWidgetProperty: 42;

    // Good
    .my-cool-widget {
      ...
    }

    // Good
    $my-cool-widget-property: 42;
    ```

* **<a name="scss-units"></a>2.5 Units**

  * Always favour `rem` units for global sizing and use `em` or `rem` for local sizing. Never use `px` unless you're working with a legacy component which isn't responsive (eg. data grids). Note that the application currently uses a 16px as its base font size.

    Need helping converting `px` to `rem`? It's as easy as `your-px-value / font-base`, eg. `30px / 16px = 1.875rem` 

    > Why? Relative sizing units are a must when building a responsive web app.
      
    ```css
    // Bad
    .widget {
      width: 16px;
      height: 30px;
    }

    // Good 
    .widget {
      width: 1rem;
      height: 1.875rem;
    }

    ```
  
  * Never specify a unit if a property's numeric value is 0. scss-lint: [`ZeroUnit`](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#zerounit)

    ```css
    // Bad
    .widget {
      margin: 0rem;
    }

    // Good 
    .widget {
      margin: 0;
    }

    ```

* **<a name="scss-colors"></a>2.6 Colors**

  * The Predix Design Systen palette and Studio palette have been implemented as SCSS variables. Please use these existing variables when specifying color properties. If a new color is required, please extend the Studio palette.

  **Predix Design System palette**: Located at `scss/px-colors-design/_settings.colors.scss`
  
  **Studio palette**: Located at `scss/px-colors-studio/_studio.colors.scss`

    ```css
    // Bad
    .btn-cta {
      color: #fff;
      background-color: #007acc; 
    }

    // Good 
    .btn-cta {
      color: $white;
      background-color: $primary-default;
    }  
    ```

* **<a name="scss-selectors"></a>2.7 Selectors**

  * Favour nested selectors offered in SCSS over duplicating selectors.

    ```css
    // Bad
    .my-widget {
      width: 3rem;
    }

    .my-widget .widget-buttons {
      width: 1rem;
    }

    // Good 
    .my-widget {
      width: 3rem;

      .widget-buttons {
        width: 1rem;
      }
    }  
    ```

  * Avoid ID selectors whenever possible. scss-lint: [`IdSelector `](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#idselector)

    > Why? ID selectors introduce an unnecessarily high level of specificity to your rule declarations, and they are not reusable.

    ```css
    // Bad
    #my-one-and-only-widget {
      ...
    }

    // Good 
    .my-nice-reusable-widget {
      ...
    }  
    ```
  
## <a name="contributing"></a>Contributing

Pull requests are welcome! If you do add, change or remove a rule, please including the changes to the linting configs in the same PR. A quick description in your PR to explain why a change is proposed is encouraged.

Just a note, changes just to suit a personal preference are unlikely to be accepted. However if a rule is missing, all things being equal, by all means go with your personal preference if that allows that given rule to be enforced consistency.

Thanks again for reading!
