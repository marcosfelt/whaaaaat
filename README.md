# whaaaaat?

A collection of common interactive command line user interfaces.

## Table of Contents

  1. [Documentation](#documentation)
    1. [Installation](#installation)
    2. [Examples](#examples)
    3. [Quickstart](#quickstart)
    4. [Question Types](#types)
    5. [Question Properties](#properties)
    6. [User Interfaces and Styles](#styles)
  2. [Windows Platform](#windows)
  3. [Support](#support)
  4. [Contributing](#contributing)
  5. [Acknowledgments](#acknowledgements)
  6. [License](#license)


## Goal and Philosophy

**`whaaaaat`** strives to be an easily embeddable and beautiful command line interface for [Python](https://python.org/). **`whaaaaat`** wants to make it easy for existing Inquirer.js users to write immersive command line applications in Python. We are convinced that its feature-set is the most complete for building immersive CLI applications. We also hope that **`whaaaaat`** proves itself useful to Python users.

**`whaaaaat`** should ease the process of
- providing *error feedback*
- *asking questions*
- *parsing* input
- *validating* answers
- managing *hierarchical prompts*

**Note:** **`whaaaaat`** provides the user interface and the inquiry session flow.
> 
If you're searching for a scaffolding utility, then check out [banana](https://github.com/finklabs/banana), the whaaaaat's sister utility.


## Documentation
<a name="documentation"></a>


### Installation
<a name="installation"></a>

Like most Python packages whaaaaat is available on [PyPi](TODO). Simply use pip to install the whaaaaat package

``` shell
pip install whaaaaat
```


### Quickstart
<a name="quickstart"></a>

Like Inquirer.js, using inquirer is structured into two simple steps:

* you define a **list of questions** and hand them to **prompt**
* promt returns a **list of answers**

```python
from __future__ import print_function, unicode_literals
from whaaaaat import prompt, print_json

questions = [
    {
        'type': 'input',
        'name': 'first_name',
        'message': 'What\'s your first name',
    }
]

answers = prompt(questions)
print_json(answers)  # use the answers as input for your app
```

A good starting point from here is probably the examples section.


### Examples
<a name="examples"></a>

Most of the examples intend to demonstrate a single question type or feature:

* bottom-bar.py
* expand.py
* list.py
* password.py
* when.py
* checkbox.py
* hierarchical.py
* pizza.py - demonstrate using different question types
* editor.py
* input.py
* rawlist.py


### Question Types
<a name="types"></a>

`questions` is a list of questions. Each question has a type.

#### List - `{type: 'list'}`

Take `type`, `name`, `message`, `choices`[, `default`, `filter`] properties. (Note that
default must be the choice `index` in the array or a choice `value`)

![List prompt](https://raw.githubusercontent.com/finklabs/whaaaaat/develop/docs/images/input-prompt.png)

---

#### Raw List - `{type: 'rawlist'}`

Take `type`, `name`, `message`, `choices`[, `default`, `filter`] properties. (Note that
default must the choice `index` in the array)

![Raw list prompt](https://raw.githubusercontent.com/finklabs/whaaaaat/develop/docs/images/raw-list.png)

---

#### Expand - `{type: 'expand'}`

Take `type`, `name`, `message`, `choices`[, `default`] properties. (Note that
default must be the choice `index` in the array. If `default` key not provided, then `help` will be used as default choice)

Note that the `choices` object will take an extra parameter called `key` for the `expand` prompt. This parameter must be a single (lowercased) character. The `h` option is added by the prompt and shouldn't be defined by the user.

See `examples/expand.py` for a running example.

![Expand prompt closed](https://raw.githubusercontent.com/finklabs/whaaaaat/develop/docs/images/expand-prompt-1.png)
![Expand prompt expanded](https://raw.githubusercontent.com/finklabs/whaaaaat/develop/docs/images/expand-prompt-2.png)

---

#### Checkbox - `{type: 'checkbox'}`

Take `type`, `name`, `message`, `choices`[, `filter`, `validate`, `default`] properties. `default` is expected to be an Array of the checked choices value.

Choices marked as `{checked: true}` will be checked by default.

Choices whose property `disabled` is truthy will be unselectable. If `disabled` is a string, then the string will be outputted next to the disabled choice, otherwise it'll default to `"Disabled"`. The `disabled` property can also be a synchronous function receiving the current answers as argument and returning a boolean or a string.

![Checkbox prompt](https://raw.githubusercontent.com/finklabs/whaaaaat/develop/docs/images/checkbox-prompt.png)

---

#### Confirm - `{type: 'confirm'}`

Take `type`, `name`, `message`[, `default`] properties. `default` is expected to be a boolean if used.

![Confirm prompt](https://raw.githubusercontent.com/finklabs/whaaaaat/develop/docs/images/confirm-prompt.png)

---

#### Input - `{type: 'input'}`

Take `type`, `name`, `message`[, `default`, `filter`, `validate`] properties.

![Input prompt](https://raw.githubusercontent.com/finklabs/whaaaaat/develop/docs/images/input-prompt.png)

---

#### Password - `{type: 'password'}`

Take `type`, `name`, `message`[, `default`, `filter`, `validate`] properties.

![Password prompt](https://raw.githubusercontent.com/finklabs/whaaaaat/develop/docs/images/password-prompt.png)

---

#### Editor - `{type: 'editor'}`

Take `type`, `name`, `message`[, `default`, `filter`, `validate`] properties

Launches an instance of the users preferred editor on a temporary file. Once the user exits their editor, the contents of the temporary file are read in as the result. The editor to use is determined by reading the $VISUAL or $EDITOR environment variables. If neither of those are present, notepad (on Windows) or vim (Linux or Mac) is used.


### Question Properties
<a name="properties"></a>

A question is a dictionary containing question related values:

* type: (String) Type of the prompt. Defaults: input - Possible values: input, confirm, list, rawlist, expand, checkbox, password, editor
* name: (String) The name to use when storing the answer in the answers hash. If the name contains periods, it will define a path in the answers hash.
* message: (String|Function) The question to print. If defined as a function, the first parameter will be the current inquirer session answers.
* default: (String|Number|Array|Function) Default value(s) to use if nothing is entered, or a function that returns the default value(s). If defined as a function, the first parameter will be the current inquirer session answers.
* choices: (Array|Function) Choices array or a function returning a choices array. If defined as a function, the first parameter will be the current inquirer session answers. Array values can be simple strings, or objects containing a name (to display in list), a value (to save in the answers hash) and a short (to display after selection) properties. The choices array can also contain a Separator.
* validate: (Function) Receive the user input and should return true if the value is valid, and an error message (String) otherwise. If false is returned, a default error message is provided.
* filter: (Function) Receive the user input and return the filtered value to be used inside the program. The value returned will be added to the Answers hash.
* when: (Function, Boolean) Receive the current user answers hash and should return true or false depending on whether or not this question should be asked. The value can also be a simple boolean.
* pageSize: (Number) Change the number of lines that will be rendered when using list, rawList, expand or checkbox.


### User Interfaces and Styles
<a name="styles"></a>

TODO


## Windows Platform
<a name="windows"></a>

**`whaaaaat`** is build on prompt_toolkit which is cross platform, and everything that you build on top should run fine on both Unix and Windows systems. On Windows, it uses a different event loop (WaitForMultipleObjects instead of select), and another input and output system. (Win32 APIs instead of pseudo-terminals and VT100.)

It's worth noting that the implementation is a "best effort of what is possible". Both Unix and Windows terminals have their limitations. But in general, the Unix experience will still be a little better.

For Windows, it's recommended to use either cmder or conemu.


## Support
<a name="support"></a>

Most of the questions are probably related to using a question type or feature. Please lookup and study the appropriate examples.

Issue on Github TODO link

For many issues like for example common Python programming issues stackoverflow might be a good place to search for an answer.  TODO link

Visit the finklabs slack channel for announcements and news. TODO link


## Contributing
<a name="contributing"></a>

Unit test Unit test are written using pytest. Please add a unit test for every new feature or bug fix.

Documentation Add documentation for every API change. Feel free to send typo fixes and better docs!

We're looking to offer good support for multiple prompts and environments. If you want to help, we'd like to keep a list of testers for each terminal/OS so we can contact you and get feedback before release. Let us know if you want to be added to the list.


## Acknowledgments
<a name="acknowledgements"></a>

Many thanks to our friends at Inquirer.js. We think they did a great job developing the tooling to support the nodejs technology.


## License
<a name="license"></a>

Copyright (c) 2016-2017 Mark Fink (twitter: @markfink) Licensed under the MIT license.
