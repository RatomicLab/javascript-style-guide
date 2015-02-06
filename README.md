# Mirego JavaScript Style Guide() {

*A mostly reasonable approach to JavaScript*

## Table of Contents

  1. [Types](#types)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Conditional Expressions & Equality](#conditional-expressions--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [Events](#events)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [License](#license)

## Types

  - **Primitives**: When you access a primitive type you work directly on its value.

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```js
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

  - **Complex**: When you access a complex type you work on a reference to its value.

    + `object`
    + `array`
    + `function`

    ```js
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#table-of-contents)**

## Objects

  - Use the literal syntax for object creation.

    ```js
    // Bad
    var item = new Object();

    // Good
    var item = {};
    ```

  - Don’t use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won’t work in IE8.

    ```js
    // Bad
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // Good
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

    If you really *must* use a reserved word, use the subscript notation:

    ```js
    // Bad
    require('some-module').default;

    // Good
    require('some-module')['default'];
    ```

  - Use readable synonyms in place of reserved words.

    ```js
    // Bad
    var superman = {
      class: 'alien'
    };

    // Bad
    var superman = {
      klass: 'alien'
    };

    // Good
    var superman = {
      type: 'alien'
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  - Use the literal syntax for array creation.

    ```js
    // Bad
    var items = new Array();

    // Good
    var items = [];
    ```

  - If you don’t know array length use Array#push.

    ```js
    var someStack = [];

    // Bad
    someStack[someStack.length] = 'abracadabra';

    // Good
    someStack.push('abracadabra');
    ```

  - When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```js
    var length = items.length;
    var itemsCopy = [];
    var index;

    // Bad
    for (index = 0; index < length; index++) {
      itemsCopy[index] = items[index];
    }

    // Good
    itemsCopy = items.slice();
    ```

  - To convert an array-like object to an array, use Array#slice.

    ```js
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      // ...
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Strings

  - Use single quotes `''` for strings.

    ```js
    // Bad
    var name = "Bob Parr";

    // Good
    var name = 'Bob Parr';

    // Bad
    var fullName = "Bob " + this.lastName;

    // Good
    var fullName = 'Bob ' + this.lastName;
    ```

  - Strings longer than 80 characters should be written across multiple lines using string concatenation. Strings on the following lines should be indented to indicate they’re part of the previous expression.

    ```js
    // Bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // Bad
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // Good
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - String interpolation should be done with backticks when using an ECMAScript 6 to ECMAScript 5 transpiler.

  ```js
  var message = `Hello ${firstName} ${lastName}`;
  ```

**[⬆ back to top](#table-of-contents)**

## Functions

  - Knowing the difference between a function declaration and a function expression

  ```js
  // Function declaration
  function foo() {
    console.log('I’m a declaration');
  }

  // Function expression
  var bar = function() {
    console.log('I’m an expression');
  };
  ```

  - Function expressions:

    ```js
    // Anonymous function expression
    var anonymous = function() {
      return true;
    };

    // Named function expression
    // Name function expression are convenient when debugging because
    // the DevTools will show their name instead of "anonymous function"
    var named = function named() {
      return true;
    };

    // Immediately-invoked function expression (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Never declare a function in a non-function block (if, while, etc). Browsers will allow you to do it, but they all interpret it differently. Assign the function to a variable instead.

  - **Note:** ECMA-262 defines a "block" as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```js
    // Bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // Good
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - Never name a parameter `arguments`, this will take precedence over the `arguments` object that is given to every function scope.

    ```js
    // Bad
    function nope(name, options, arguments) {
      // ...
    }

    // Good
    function yup(name, options, args) {
      // ...
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Properties

  - Use dot notation when accessing properties.

    ```js
    var luke = {
      jedi: true,
      age: 28
    };

    // Bad
    var isJedi = luke['jedi'];

    // Good
    var isJedi = luke.jedi;
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ back to top](#table-of-contents)**

## Variables

  - Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace.

    ```js
    // Bad
    superPower = new SuperPower();

    // Good
    var superPower = new SuperPower();
    ```

  - Use one `var` declaration per variable. It’s easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs.

    ```js
    // Bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // Bad
    // (compare to above, and try to spot the mistake)
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // Good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```js
    // Bad
    var index, length, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // Bad
    var index;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var length;

    // Good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var index;
    ```

  - Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

    ```js
    // Bad
    function() {
      test();

      //...

      var name = getName();

      if (name === 'test') return false;

      return name;
    }

    // Good
    function() {
      var name = getName();

      test();

      //...

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // Bad
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // Good
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Hoisting

  - Variable declarations get hoisted to the top of their scope, their assignment does not.

    ```js
    // We know this wouldn’t work
    // (assuming there is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // Creating a variable declaration after you reference the variable will work due to
    // variable hoisting. Note: the assignment value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable declaration to the top of the scope,
    // which means our example could be rewritten as:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Anonymous function expressions hoist their variable name, but not the function assignment.

    ```js
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Named function expressions hoist the variable name, not the function name or the function body.

    ```js
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // The same is true when the function name is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Function declarations hoist their name and the function body.

    ```js
    function example() {
      superPower(); // => Logs 'Flying'

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ back to top](#table-of-contents)**

## Conditional Expressions & Equality

  - Use `===` and `!==` over `==` and `!=`.

  - Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to `true`
    + `undefined` evaluates to `false`
    + `null` evaluates to `false`
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to `false` if `+0`, `-0`, or `NaN`, otherwise `true`
    + **Strings** evaluate to `false` if empty, otherwise `true`

    ```js
    if ([0]) {
      // This is executed
      // An array is an object, objects evaluate to `true`
    }
    ```

  - Use shortcuts.

    ```js
    // Bad
    if (name !== '') {
      // ...
    }

    // Good
    if (name) {
      // ...
    }

    // Bad
    if (collection.length > 0) {
      // ...
    }

    // Good
    if (collection.length) {
      // ...
    }
    ```

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ back to top](#table-of-contents)**

## Blocks

  - Use braces with all multi-line blocks. You can omit braces when a block contains only one statement but still fits in a 80-characters line.

    ```js
    // Bad
    if (test)
      return false;

    // Good
    if (test) return false;

    // Good
    if (test) {
      return false;
    }

    // Bad
    function() { return false; }

    // Good
    function() {
      return false;
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Comments

  - Comments should not be common place in your code, we aim to write code that is simple and understandable at a glance. When we do write comments, it is to:
    + put emphasis on a hack, explain why it is there and explain how it works;
    + explain complex logic;
	+ explain domain logic which might not be obvious;
	+ separate code blocks (mainly configuration blocks);

  - Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you’re pointing out a problem that needs to be revisited, or if you’re suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME: need to figure this out` or `TODO: need to implement`.

  - Use `// FIXME` to annotate problems.

    ```js
    function Calculator() {
      // FIXME shouldn’t use a global here
      total = 0;

      return this;
    }
    ```

  - Use `// TODO` to annotate solutions to problems.

    ```js
    function Calculator() {
      // TODO total should be configurable by an options param
      this.total = 0;

      return this;
    }
  ```

**[⬆ back to top](#table-of-contents)**

## Whitespace

  - Use soft tabs set to 2 spaces.

    ```js
    // Bad
    function() {
    ∙∙∙∙var name;
    }

    // Bad
    function() {
    ∙var name;
    }

    // Good
    function() {
    ∙∙var name;
    }
    ```

  - Place 1 space before the leading brace.

    ```js
    // Bad
    function test(){
      console.log('test');
    }

    // Good
    function test() {
      console.log('test');
    }

    // Bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // Good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - Set off operators with spaces.

    ```js
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - End files with a single newline character.

    ```js
    // Bad
    (function(global) {
      // ...
    })(this);
    ```

    ```js
    // Bad
    (function(global) {
      // ...
    })(this);↵
    ↵
    ```

    ```js
    // Good
    (function(global) {
      // ...
    })(this);↵
    ```

  - Use indentation when making long method chains. Use a leading dot, which
    emphasizes that the line is a method call, not a new statement.

    ```js
    // Bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // Bad
    $('#items').
      find('selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // Good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // Bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

**[⬆ back to top](#table-of-contents)**

## Commas

  - Leading commas: **Nope.**

    ```js
    // Bad
    var story = [
        once
      , upon
      , aTime
    ];

    // Good
    var story = [
      once,
      upon,
      aTime
    ];

    // Bad
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // Good
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it’s in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```js
    // Bad
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // Good
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[⬆ back to top](#table-of-contents)**

## Semicolons

  - **Always.**

    ```js
    // Bad
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // Good
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // Good (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ back to top](#table-of-contents)**

## Type Casting & Coercion

  - Perform type coercion at the beginning of the statement.

  - Strings:

    ```js
    //  => this.reviewScore = 9;

    // Bad
    var totalScore = this.reviewScore + '';

    // Good
    var totalScore = '' + this.reviewScore;

    // Bad
    var totalScore = '' + this.reviewScore + ' total score';

    // Good
    var totalScore = this.reviewScore + ' total score';
    ```

  - Use `parseInt` and `parseFloat` for Numbers. When using `parseInt` always specify a radix.

    ```js
    var integerValue = '4';
    var floatValue = '4.5';

    // Bad
    var value = new Number(integerValue);

    // Bad
    var value = + integerValue;

    // Bad
    var value = integerValue >> 0;

    // Bad
    var value = parseInt(integerValue);

    // Good
    var value = Number(integerValue);

    // Good
    var value = parseInt(integerValue, 10);

    // Good
    var value = parseFloat(floatValue);
    ```

  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```js
    // Good

    // `parseInt` was the reason my code was slow. Bitshifting the String
    // to coerce it to a Number made it a lot faster.
    var value = inputValue >> 0;
    ```

  - **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. Largest signed 32-bit Int is 2,147,483,647:

    ```js
    2147483647 >> 0 // => 2147483647
    2147483648 >> 0 // => -2147483648
    2147483649 >> 0 // => -2147483647
    ```

  - Booleans:

    ```js
    var age = 0;

    // Bad
    var hasAge = new Boolean(age);

    // Good
    var hasAge = Boolean(age);

    // Good
    var hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**


## Naming Conventions

  - Avoid single letter names. Be descriptive with your naming. We prefer readable and understandable code to hand-minified code. Don’t worry about the few extra bytes, minifiers and gzip take care of that for us.

    ```js
    // Bad
    function q() {
      // ...
    }

    // Good
    function query() {
      // ...
    }
    ```

  - Use camelCase when naming objects, functions, and instances.

    ```js
    // Bad
    var OBJEcttsssss = {};

    var this_is_my_object = {};

    function c() {}

    var u = new user({
      name: 'Bob Parr'
    });

    // Good
    var thisIsMyObject = {};

    function thisIsMyFunction() {}

    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Use PascalCase when naming constructors or classes.

    ```js
    // Bad
    function user(options) {
      this.name = options.name;
    }

	class user {
      constructor(options) {
        this.name = options.name;
      }
    }

    // Good
    function User(options) {
      this.name = options.name;
    }

    class User {
      constructor(options) {
        this.name = options.name;
      }
    }
    ```

  - Use UPPER\_CASE\_SNAKE\_CASE when naming constants.

  ```js
  var API_URL = '...';
  ```

  - Use a leading underscore `_` when naming private properties.

    ```js
    // Bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // Good
    this._firstName = 'Panda';
    ```

  - When saving a reference to `this` use `self`.

    ```js
    // Bad
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }

    // Bad
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // Good
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }
    ```

  - Name your functions expressions. This is helpful for stack traces.

    ```js
    // Bad
    var log = function(msg) {
      console.log(msg);
    };

    // Good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  - **Note:** IE8 and below exhibit some quirks with named function expressions. See [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/) for more info.

**[⬆ back to top](#table-of-contents)**


## Accessors

  - Accessor functions for properties are not required.

  - If you do make accessor functions use `getValue()` and `setValue('hello')`.

    ```js
    // Bad
    dragon.age();

    // Good
    dragon.getAge();

    // Bad
    dragon.age(25);

    // Good
    dragon.setAge(25);
    ```

  - If the property is a boolean, use `isValue()` or `hasValue()`.

    ```js
    // Bad
    if (!dragon.age()) {
      return false;
    }

    // Good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - It’s okay to create `get()` and `set()` functions, but be consistent.

    ```js
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Classes

  - When possible use an ECMAScript 6 to ECMAScript 5 transpiler and write `class`es.

  ```js
  class Jedi {
    constructor() {
      console.log('new jedi');
    }
  }
  ```

  - If you must write classes the old way, assign methods to the prototype object instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you’ll overwrite the base!

    ```js
    function Jedi() {
      console.log('new jedi');
    }

    // Bad
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // Good
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Methods can return `this` to help with method chaining.

    ```js
    // Bad
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // Good
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - It’s okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```js
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Events

  - When attaching data payloads to events (whether DOM events or custom events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

    ```js
    // Bad
    $(this).trigger('listingUpdated', listing.id);
    ...

    $(this).on('listingUpdated', function(event, listingId) {
      // do something with listingId
    });
    ```

    prefer:

    ```js
    // Good
    $(this).trigger('listingUpdated', { listingId : listing.id });
    ...

    $(this).on('listingUpdated', function(event, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ back to top](#table-of-contents)**

## Modules

  - Use ECMAScript 6 module syntax when possible.

  ```js
  import Ember from 'ember';

  export default Ember.Component.extend();
  ```

**[⬆ back to top](#table-of-contents)**


## jQuery

  - Prefix jQuery object variables with a `$`.

    ```js
    // Bad
    var sidebar = $('.sidebar');

    // Good
    var $sidebar = $('.sidebar');
    ```

  - Cache DOM lookups.

    ```js
    // Bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // Good
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - Use `find` with scoped jQuery object queries.

    ```javascript
    // Bad
    $('ul', '.sidebar').hide();

    // Bad
    $('.sidebar').find('ul').hide();

    // Good
    $('.sidebar ul').hide();

    // Good
    $('.sidebar > ul').hide();

    // Best
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#table-of-contents)**

## ECMAScript versions compatibility

  - Refer to [Kangax](https://twitter.com/kangax/)’s compatibility tables

    + ES5: [http://kangax.github.io/compat-table/es5/](http://kangax.github.io/compat-table/es5/).
    + ES6: [http://kangax.github.io/compat-table/es6/](http://kangax.github.io/compat-table/es6/).

**[⬆ back to top](#table-of-contents)**

## Testing

  - **Yup.** You should setup your project with:
    + [testem](https://github.com/airportyh/testem)
    + [Mocha](http://mochajs.org/)
    + [Chai](http://chaijs.com/)

  - If possible, setup your project to run your specs on TravisCI along with style validations.

**[⬆ back to top](#table-of-contents)**

## Resources

**Read This**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Tools**

  - Code Style Linters
    + [Phare](https://github.com/mirego/phare)
    + [JSHint](http://www.jshint.com/) - [Mirego Style .jshintrc](https://github.com/mirego/javascript-style-guide/blob/master/linters/.jshintrc)
    + [JSCS](http://jscs.info/) - [Mirego Style .jscs.json](https://github.com/mirego/javascript-style-guide/blob/master/linters/.jscs.json)

**[⬆ back to top](#table-of-contents)**

## License

(The MIT License)

Copyright (c) 2014 Airbnb
Copyright (c) 2015 Mirego

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

# };
