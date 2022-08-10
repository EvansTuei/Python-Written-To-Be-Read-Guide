# Python Guide

This is a Data' Python Style Guide.

## <a name='TOC'>Table of Contents</a>

1. [Introduction](#introduction)
2. [Style Rules](#rules)
3. [Conventions](#conventions)
4. [Linting](#linting)

## <a name='introduction'>1. Introduction</a>

**Code is read much more than it is written**

Python is a beautiful and intentionally readable language, however this alone is not enough to ensure that we are all on the same page and which generally

The core goals of this style guide are:

- Optimize for reading not writing
- Consistency that allows for automation

The guide is separated into several sections of related guidelines with the rationale behind the guidelines added (if it&rsquo;s omitted we&rsquo;ve assumed it&rsquo;s pretty obvious).

What follows has not been grabbed from thin air but is based heavily on the [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html) and [PEP 8](https://pep8.org/).

This style guide will no doubt evolve over time as additional conventions are identified and past conventions are rendered obsolete by feedback and the language itself.

## <a name='rules'>2. Style Rules</a>

### 2.1 Tabs or Spaces?

Use only spaces for indentation. No hard tabs.

### 2.2. Indentation

Indent your code blocks with 4 spaces.

Never use tabs or mix tabs and spaces. In cases of implied line continuation, you should align wrapped elements either vertically, as per the examples in the line length section; or using a hanging indent of 4 spaces, in which case there should be nothing after the open parenthesis or bracket on the first line.

```python
    # Aligned with opening delimiter
    foo = long_function_name(var_one, var_two,
                            var_three, var_four)
    meal = (spam,
            beans)

    # Aligned with opening delimiter in a dictionary
    foo = {
        long_dictionary_key: value1 +
                            value2,
        ...
    }

    # 4-space hanging indent; nothing on first line
    foo = long_function_name(
        var_one, var_two, var_three,
        var_four)
    meal = (
        spam,
        beans)

    # 4-space hanging indent in a dictionary
    foo = {
        long_dictionary_key:
            long_dictionary_value,
        ...
    }
```

### 2.3 Line length

Maximum line length is 80 characters.

Explicit exceptions to the 80 character limit:

- Long import statements.
- URLs, pathnames, or long flags in comments.
- Long string module level constants not containing whitespace that would be inconvenient to split across lines such as URLs or pathnames.
- Pylint disable comments. (e.g.: `# pylint: disable=invalid-name`)

Make use of Python&rsquo;s implicit line joining inside parentheses, brackets and braces. If necessary, you can add an extra pair of parentheses around an expression.

e.g.

```python
foo_bar(self, width, height, color='black', design=None, x='foo',
             emphasis=None, highlight=0)

     if (width == 0 and height == 0 and
         color == 'red' and emphasis == 'strong'):
```

When a literal string won&rsquo;t fit on a single line, use parentheses for implicit line joining.

```python
x = ('This will build a very long long '
     'long long long long long long string')
```

### 2.4 Newlines

Don&rsquo;t include newlines between areas of different indentation (such as around class or module bodies). [link]

```python
# bad
class SampleClass(object):

    def parse_records(records):
        result = {}
        for r in records:

            record = parse_record(r)

            for col in insert_columns:

                if col in result:
                    result[col].append(record[col])
                else:
                    result[col] = []
                    result[col].append(
                        record[col]) if col in record else result[col].append(None)
```

```python
# good
class SampleClass(object):
    def parse_records(records):
        result = {}
        for r in records:
            record = parse_record(r)
            for col in insert_columns:
                if col in result:
                    result[col].append(record[col])
                else:
                    result[col] = []
                    result[col].append(
                        record[col]) if col in record else result[col].append(None)
```

#### 2.4.1 Include one, but no more than one, new line between methods

```python
def sum(x, y):
    x + y

def subtract(x, y):
    x - y

def multiply(x, y):
    x * y
```

#### 2.4.2 Use a single empty line to break between statements to break up methods into logical paragraphs internally.

```python
def transformorize_car(options):
  car = manufacture(options)
  t = transformer(robot, disguise)

  car.after_market_mod!
  t.transform(car)
  car.assign_cool_name!

  fleet.add(car)
  car
```

End each file with a newline. Don't include multiple newlines at the end of a file.

### 2.5 True/False evaluation

Python evaluates certain values as `False` when in a boolean context. A quick &rdquo;rule of thumb&ldquo; is that all &rdquo;empty&ldquo; values are considered false, so `0`, `None`, `[]`, `{}`, `''` all evaluate as false in a boolean context.

Use the &rdquo;implicit&ldquo; false if possible, e.g., `if foo:` rather than if `foo != []:`. There are a few caveats that you should keep in mind though:

- Always use if foo is None: (or is not None) to check for a None value-e.g., when testing whether a variable or argument that defaults to None was set to some other value. The other value might be a value that&rsquo;s false in a boolean context!

- Never compare a boolean variable to `False` using `==.` Use `if not x:` instead. If you need to distinguish `False` from `None` then chain the expressions, such as `if not x and x is not None:`.

- For sequences (strings, lists, tuples), use the fact that empty sequences are false, so `if seq:` and `if not seq:` are preferable to `if len(seq):` and `if not len(seq):` respectively.

- When handling integers, implicit false may involve more risk than benefit (i.e., accidentally handling `None` as `0`). You may compare a value which is known to be an integer (and is not the result of `len()`) against the integer 0.

Good:

```python
if not users:
    print('no users')

if foo == 0:
    self.handle_zero()

if i % 10 == 0:
    self.handle_multiple_of_ten()

def f(x=None):
    if x is None:
        x = []
```

Bad:

```python
if len(users) == 0:
    print('no users')

    if foo is not None and not foo:
        self.handle_zero()

    if not i % 10:
        self.handle_multiple_of_ten()

    def f(x=None):
        x = x or []
```

- Note that '0' (i.e., 0 as string) evaluates to true.

### 2.6 Comments and Docstrings

Be sure to use the right style for class, function, method docstrings and inline comments.

#### 2.6.1 Docstrings

Python uses docstrings to document code. A docstring is a string that is the first statement in a package, module, class or function. These strings can be extracted automatically through the `__doc__` member of the object and are used by `pydoc`.
Always use the three double-quote """ format for docstrings (per [PEP 257](https://www.google.com/url?sa=D&q=http://www.python.org/dev/peps/pep-0257/)).

A docstring should be organized as a summary line (one physical line) terminated by a period, question mark, or exclamation point, followed by a blank line, followed by the rest of the docstring starting at the same cursor position as the first quote of the first line.

Examples for class, function, method docstrings and inline comments follow.

#### 2.6.2 Classes

Classes should have a docstring below the class definition describing the class. If your class has public attributes, they should be documented here in an Attributes section and follow the same formatting as a function’s Args section.

```python
class SampleClass(object):
    """Summary of class here.

    Longer class information....
    Longer class information....

    Attributes:
        likes_spam: A boolean indicating if we like SPAM or not.
        eggs: An integer count of the eggs we have laid.
    """

    def __init__(self, likes_spam=False):
        """Inits SampleClass with blah."""
        self.likes_spam = likes_spam
        self.eggs = 0

    def public_method(self):
        """Performs operation blah."""
```

#### 2.6.3 Functions and Methods

A function must have a docstring, unless it meets all of the following criteria:

- not externally visible
- very short
- obvious

A docstring should give enough information to write a call to the function without reading the function&rsquo;s code. The docstring should be descriptive-style (`"""Fetches rows from the warehouse."""`) rather than imperative-style (`"""Fetch rows from the warehouse."""`).

A docstring should describe the function&rsquo;s calling syntax and its semantics, not its implementation. For tricky code, comments alongside the code are more appropriate than using docstrings.

Aspects of a function should be documented in special sections, as listed below. Each section begins with a heading line, which ends with a colon. All sections other than the heading should maintain a hanging indent of two or four spaces (be consistent within a file).

<a id="doc-function-args"></a>
[_Args:_](#doc-function-args)
List each parameter by name. A description should follow the name, and be separated by a colon and a space. If the description is too long to fit on a single 80-character line, use a hanging indent of 2 or 4 spaces (be consistent with the rest of the file).

    The description should include required type(s) if the code does not contain a corresponding type annotation. If a function accepts `*foo` (variable length argument lists) and/or `**bar` (arbitrary keyword arguments), they should be listed as `*foo` and `**bar`.

<a id="doc-function-returns"></a>
[_Returns:_ (or _Yields:_ for generators)](#doc-function-returns)
Describe the type and semantics of the return value. If the function only returns None, this section is not required. It may also be omitted if the docstring starts with Returns or Yields (e.g. `"""Returns row from tbl_loan_application table as a list containing dicts."""`) and the opening sentence is sufficient to describe return value.

<a id="doc-function-raises"></a>
[_Raises:_](#doc-function-raises)
List all exceptions that are relevant to the interface. You should not document exceptions that get raised if the API specified in the docstring is violated (because this would paradoxically make behavior under violation of the API part of the API).

Sections can be omitted in cases where the function's name and signature are informative enough that it can be aptly described using a one-line docstring.

```python
def read_crb(limit='ALL'):
    """Fetches rows from the CRB table in the warehouse.

    Args:
        limit: The number of rows to be return returned.

    Returns:
        A list containing dicts mapping keys to the corresponding table row data
        fetched. Each row is represented as a tuple of strings. For
        example:

        {'id': 1,
         'report_date': '2018-08-15 00:00:00',
         'full_name': 'HOPE PROMISE AHADI'}

    Raises:
        ProgrammingError: An error occurred ...
    """
    cur = crb_conn.cursor()
    cur.execute(f"""
        SELECT response_data FROM public.tbl_requests
        WHERE response_data NOT IN ('null', '\"Missing query_type\"', '{"responseCode":402}')
        LIMIT {limit}
        """)
    results = cur.fetchall()
    data = [json.loads(row[0]) for row in results]
    cur.close()
    crb_conn.close()
    return data

```

### 2.7 Reading From a File

Use the with open syntax to read from files. This will automatically close files for you.

Bad:

```python
f = open('file.txt')
a = f.read()
print a
f.close()
```

Good:

```python
with open('file.txt') as f:
    for line in f:
        print line
```

## <a name='convetions'>3. Conventions</a>

Here are some conventions you should follow to make your code easier to read.

### 3.1 TODO Comments

Use TODO comments for code that is temporary, a short-term solution, or good-enough but not perfect.

A TODO comment begins with the string TODO in all caps and a parenthesized name, e-mail address, or other identifier of the person or issue with the best context about the problem. This is followed by an explanation of what there is to do.

```python
# TODO(kl@gmail.com): Use a "*" here for string repetition.
# TODO(Zeke) Change this to use relations.
```

The purpose is to have a consistent TODO format that can be searched to find out how to get more details. A TODO is not a commitment that the person referenced will fix the problem. Thus when you create a TODO, it is almost always your name that is given.

If your TODO is of the form &rdquo;At a future date do something&ldquo; make sure that you either include a very specific date (“Fix by November 2009”) or a very specific event (&rdquo;Remove this code when all clients can handle XML responses.&ldquo;).

### 3.2 Naming

`module_name`, `package_name`, `ClassName`, `method_name()`, `ExceptionName`, `function_name()`, `GLOBAL_CONSTANT_NAME`, `global_var_name`, `instance_var_name`, `function_parameter_name`, `local_var_name`.

Function names, variable names, and filenames should be descriptive; eschew abbreviation.
In particular, do not use abbreviations that are ambiguous or unfamiliar to readers outside your project, and do not abbreviate by deleting letters within a word.

e.g. don't do this `all_cus_crb_rcrds`

Python filenames must have a `.py` extension and must not contain dashes (`-`).
This allows them to be imported and unittested.

### 3.3 Punctuation, Spelling, and Grammar

Pay attention to punctuation, spelling, and grammar; it is easier to read well-written comments than badly written ones.

Comments should be as readable as narrative text, with proper capitalization and punctuation. In many cases, complete sentences are more readable than sentence fragments. Shorter comments, such as comments at the end of a line of code, can sometimes be less formal, but you should be consistent with your style.

Although it can be frustrating to have a code reviewer point out that you are using a comma when you should be using a semicolon, it is very important that source code maintain a high level of clarity and readability. Proper punctuation, spelling, and grammar help with that goal.

### 3.4 Main

Even a file meant to be used as an executable should be importable.
Import should not have the side effect of executing the program's main functionality.

The main functionality should be in a `main()` function.

In Python, `pydoc` as well as unit tests require modules to be importable. Your code should always check `if __name__ == '__main__'` before executing your main program so that the main program is not executed when the module is imported.

```python
def main():
    ...

if __name__ == '__main__':
    main()
```

All code at the top level will be executed when the module is imported. Be careful not to call functions, create objects, or perform other operations that should not be executed when the file is being `pydoc`ed.

### 3.5 Function length

Prefer small and focused functions.

Sometimes long functions are sometimes appropriate, which is why no hard limit is placed on function length. If a function exceeds about 40 lines, think about whether it can be broken up without harming the structure of the program.

Remeber, even if your long function works perfectly now, someone modifying it in a few months may add new behavior. This could result in bugs that are hard to find. Keeping your functions short and simple makes it easier for other people to read and modify your code.

Do not be intimidated by modifying existing code: if working with a long function proves to be difficult and you find that errors are hard to debug, or you want to use a piece of it in several different contexts, consider breaking up the function into smaller and more manageable pieces.

## <a name='linting'>4. Linting</a>

Use [pylint](https://www.pylint.org/) which is a python linter which checks the source code and also acts as a bug and quality checker.

## 5. Some parting words

> I&rsquo;m confident to say that if you want to grow in a profession, consistency is the key&hellip; > <br>
> I&rsquo;m strict about my work goals and training.
> <br> > &mdash; Eliud Kipchoge

_BE CONSISTENT_.

If you're editing code, take a few minutes to look at the code around you and determine its style. If they use spaces around all their arithmetic operators, you should too. The point of having style guidelines is to have a common vocabulary of coding so people can concentrate on what you're saying rather than on how you're saying it.

We present global style rules here so people know the vocabulary, but local style is also important. If code you add to a file looks drastically different from the existing code around it, it throws readers out of their rhythm when they go to read it. Avoid this.
