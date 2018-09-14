![Predix Studio logo](https://www.predix.io/assets/images/landing/studio_logo_2x.png)

# Style Guide
###### For JavaScript and SCSS

Targeting **Predix Studio** and **Predix App Engine** development. This references a subset of AirBnB's ES5 and ES6 style guides, tailored to suit Studio and App Engine's front-end stack and development patterns. SCSS style guide references a subset of AirBnB's and codeguide.co's SCSS and CSS style guides in addition to our guidelines.

## Table of Contents
1. [JavaScript Style Guide](#javascript-style-guide)
	1. [Introduction](#javascript-intro)
		1. [Linting](#javascript-linting)
		2. [Enforcement](#javascript-enforcement)
		3. [A note on module/class patterns](#javascript-patterns)
	2. [Glossary](#javascript-glossary)
	3. [Whitespace](#javascript-whitespace)
	4. [Naming Conventions](#javascript-naming)
	5. [Variables and References](#javascript-variables)
	6. [Quotes](#javascript-quotes)
	7. [Comments](#javascript-comments)
	8. [Comparison Operators & Equality](#javascript-operators)
	9. [jQuery](#javascript-jquery)
	10. [Classes & Constructors](#javascript-classes) (ES6 extensions only)
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

* **<a name="javascript-whitespace"></a>1.3 Whitespace**

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

* **<a name="javascript-naming"></a>1.4 Naming Conventions**

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

* **<a name="javascript-variables"></a>1.5 Variables and References**

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

* **<a name="javascript-quotes"></a>1.6 Quotes**

  * Use single quotes `''` for strings. eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)

    ```javascript
    // bad
    const name = "This is not as cool, but still a string";

    // good
    const name = 'This is an awesome string of many characters';
    ```
    
* **<a name="javascript-comments"></a>1.7 Comments**

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
   
* **<a name="javascript-operators"></a>1.8 Comparison Operators & Equality**

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
   
* **<a name="javascript-jquery"></a>1.9 jQuery**

  ##### NOTE: No jQuery guidelines have linting support. Enforcement of these guidelines is manual only.
	
  * Prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');
    ```

  * Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```
    
  * Utilise the new $.fn.on and $.fn.off method of attaching and detaching event handlers rather than the older deprecated methods ($.fn.bind, $.fn.unbind, $.fn.delegate, $.fn.undelegate) 
 
    ```javascript
    // bad
    $el.bind('click', function () { ... });
    $el.unbind('click');

    // good
    $el.on('click', function () { ... });
    $el.off('click');
    ```

  * Do not use the 'ready' event in Jquery - it is not always clear on when it will fire. Utilize the new thenable object $.ready - it will fire consistently in both AJAX and DOM Loaded events.

    ```javascript
    // bad
    $(...).on('ready', function () {
      ...
    });

    // good
    $.when($.ready).then( function () {
      ...
    });

    // also good
    $( function () {
      ... 
    });
    ```
  
* **<a name="javascript-classes"></a>1.10 Classes and Constructors**

  * Always use `class`. Avoid manipulating `prototype` directly.

    > Why? `class` syntax is more concise and easier to reason about.

    ```javascript
    // bad
    function Queue(contents = []) {
      this.queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    };

    // good
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents];
      }
      pop() {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
      }
    }
    ```

  * Use `extends` for inheritance.

    > Why? It is a built-in way to inherit prototype functionality without breaking `instanceof`.

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this.queue[0];
    };

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0];
      }
    }
    ```

  * Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
          return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  * Classes have a default constructor if one is not specified. An empty constructor function or one that just delegates to a parent class is unnecessary. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // bad
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // bad
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // good
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  * Avoid duplicate class members. eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > Why? Duplicate class member declarations will silently prefer the last one - having duplicates is almost certainly a bug.

    ```javascript
    // bad
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
     }

    // good
    class Foo {
      bar() { return 1; }
    }

    // good
    class Foo {
      bar() { return 2; }
    }
    ```
    
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
