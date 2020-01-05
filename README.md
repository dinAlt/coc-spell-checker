# Spelling Checker for (Neo)vim

> fork from [vscode-spell-checker](https://github.com/streetsidesoftware/vscode-spell-checker)
and [commit](c839c736bf8b2ba4e412a65afae50a52caa1dd7b)

A basic spell checker that works well with camelCase code.

The goal of this spell checker is to help catch common spelling errors while keeping
the number of false positives low.

## Functionality

Load a TypeScript, JavaScript, Text, etc. file.  Words not in the dictionary
files will have a squiggly underline.

![screenshot](https://user-images.githubusercontent.com/5492542/71676618-9e86f380-2dbb-11ea-9838-2291e895c7ff.png)

To see the list of suggestions:

![screenshot](https://user-images.githubusercontent.com/5492542/71676668-bc545880-2dbb-11ea-8bcc-4bd9140ef959.png)

Paste below configurations to your `init.vim` or `.vimrc`

- Remap for do codeAction of selected region

  ``` vim
  vmap <leader>a <Plug>(coc-codeaction-selected)
  nmap <leader>a <Plug>(coc-codeaction-selected)
  ```

Then positioning the cursor in the word, any of the following should
display the list of suggestions:

- `<leader>aap` for current paragraph
- `<leader>aw` for current word

Or use the [coc-actions](https://github.com/iamcco/coc-actions)

![image](https://user-images.githubusercontent.com/5492542/71774253-78717700-2fa6-11ea-9629-1e88d0b5114e.png)

## Install

`:CocInstall coc-spell-checker`

### Commands and Configurations

- [commands](https://github.com/iamcco/coc-spell-checker/blob/master/package.json#L30)
- [configurations](https://github.com/iamcco/coc-spell-checker/blob/master/package.json#L125)

## Supported Languages

- English (US)
- English (GB) - turn on by changing `"cSpell.language": "en"` to
`"cSpell.language": "en-GB"`

## Add-On Dictionaries

- [catalan](https://github.com/iamcco/coc-cspell-dicts)
- [czech](https://github.com/iamcco/coc-cspell-dicts)
- [danish](https://github.com/iamcco/coc-cspell-dicts)
- [dutch](https://github.com/iamcco/coc-cspell-dicts)
- [french](https://github.com/iamcco/coc-cspell-dicts)
- [french-reforme](https://github.com/iamcco/coc-cspell-dicts)
- [german](https://github.com/iamcco/coc-cspell-dicts)
- [greek](https://github.com/iamcco/coc-cspell-dicts)
- [italian](https://github.com/iamcco/coc-cspell-dicts)
- [persian](https://github.com/iamcco/coc-cspell-dicts)
- [polish](https://github.com/iamcco/coc-cspell-dicts)
- [portuguese](https://github.com/iamcco/coc-cspell-dicts)
- [portuguese-brazilian](https://github.com/iamcco/coc-cspell-dicts)
- [russian](https://github.com/iamcco/coc-cspell-dicts)
- [spanish](https://github.com/iamcco/coc-cspell-dicts)
- [swedish](https://github.com/iamcco/coc-cspell-dicts)
- [turkish](https://github.com/iamcco/coc-cspell-dicts)
- [ukrainian](https://github.com/iamcco/coc-cspell-dicts)
- [medical-terms](https://github.com/iamcco/coc-cspell-dicts)

## Enabled File Types

- AsciiDoc
- C, C++
- C#
- css, less, scss
- Elixir
- Go
- Html
- Java
- JavaScript
- JSON / JSONC
- LaTex
- Markdown
- PHP
- PowerShell
- Pug / Jade
- Python
- reStructuredText
- Rust
- Scala
- Text
- TypeScript
- YAML

## How it works with camelCase

The concept is simple, split camelCase words before checking them against
a list of known English words.

- camelCase -> camel case
- HTMLInput -> html input -- Notice that the `I` is associated with `Input` and not `HTML`
- snake_case_words -> snake case words
- camel2snake -> camel snake -- (the 2 is ignored)

### Special case will ALL CAPS words

There are a few special cases to help will common spelling practices for
ALL CAPS words.

Trailing `s`, `ing`, `ies`, `es`, `ed` are kept with the previous word.

- CURLs -> curls -- trailing `s`
- CURLedRequest -> curled request -- trailing `ed`

## Things to note

- This spellchecker is case insensitive.  It will not catch errors like
english which should be English.
- The spellchecker uses a local word dictionary.  It does not send anything
outside your machine.
- The words in the dictionary can and do contain errors.
- There are missing words.
- Only words longer than 3 characters are checked.  "jsj" is ok, while "jsja"
is not.
- All symbols and punctuation are ignored.

## In Document Settings

It is possible to add spell check settings into your source code.
This is to help with file specific issues that may not be applicable to the
entire project.

All settings are prefixed with `cSpell:` or `spell-checker:`.

- `disable` -- turn off the spell checker for a section of code.
- `enable` -- turn the spell checker back on after it has been turned off.
- `ignore` -- specify a list of words to be ignored.
- `words` -- specify a list of words to be considered correct and will appear
in the suggestions list.
- `ignoreRegExp` -- Any text matching the regular expression will NOT be checked
for spelling.
- `includeRegExp` -- Only text matching the collection of includeRegExp will be checked.
- `enableCompoundWords` / `disableCompoundWords` -- Allow / disallow words
like: "stringlength".

### Enable / Disable checking sections of code

It is possible to disable / enable the spell checker by adding comments to your code.

#### Disable Checking

- `/* cSpell:disable */`
- `/* spell-checker: disable */`
- `/* spellchecker: disable */`
- `/* cspell: disable-line */`
- `/* cspell: disable-next-line */`

#### Enable Checking

- `/* cSpell:enable */`
- `/* spell-checker: enable */`
- `/* spellchecker: enable */`

#### Example

``` javascript

// cSpell:disable
const wackyWord = ['zaallano', 'wooorrdd', 'zzooommmmmmmm'];
/* cSpell:enable */

// Nest disable / enable is not Supported

// spell-checker:disable
// It is now disabled.

var liep = 1;

/* cspell:disable */
// It is still disabled

// cSpell:enable
// It is now enabled

const str = "goededag";  // <- will be flagged as an error.

// spell-checker:enable <- doesn't do anything

// cSPELL:DISABLE <-- also works.

// if there isn't an enable, spelling is disabled till the end of the file.
const str = "goedemorgen";  // <- will NOT be flagged as an error.

```
<!--- cSpell:enable -->

### Ignore

Ignore allows you the specify a list of words you want to ignore within the document.

```javascript
// cSpell:ignore zaallano, wooorrdd
// cSpell:ignore zzooommmmmmmm
const wackyWord = ['zaallano', 'wooorrdd', 'zzooommmmmmmm'];
```

**Note:** words defined with `ignore` will be ignored for the entire file.

### Words

The words list allows you to add words that will be considered correct and will be used as suggestions.

```javascript
// cSpell:words woorxs sweeetbeat
const companyName = 'woorxs sweeetbeat';
```

**Note:** words defined with `words` will be used for the entire file.

### Enable / Disable compound words

In some programing language it is common to glue words together.

```c
// cSpell:enableCompoundWords
char * errormessage;  // Is ok with cSpell:enableCompoundWords
int    errornumber;   // Is also ok.
```

**Note:** Compound word checking cannot be turned on / off in the same file.
The last setting in the file determines the value for the entire file.

### Excluding and Including Text to be checked.

By default, the entire document is checked for spelling.
`cSpell:disable`/`cSpell:enable` above allows you to block off sections of the document.
`ignoreRegExp` and `includeRegExp` give you the ability to ignore or include patterns of text.
By default the flags `gim` are added if no flags are given.

The spell checker works in the following way:

1. Find all text matching `includeRegExp`
2. Remove any text matching `excludeRegExp`
3. Check the remaining text.

#### Exclude Example

```javascript
// cSpell:ignoreRegExp 0x[0-9a-f]+     -- will ignore c style hex numbers
// cSpell:ignoreRegExp /0x[0-9A-F]+/g  -- will ignore upper case c style hex numbers.
// cSpell:ignoreRegExp g{5} h{5}       -- will only match ggggg, but not hhhhh or 'ggggg hhhhh'
// cSpell:ignoreRegExp g{5}|h{5}       -- will match both ggggg and hhhhh
// cSpell:ignoreRegExp /g{5} h{5}/     -- will match 'ggggg hhhhh'
/* cSpell:ignoreRegExp /n{5}/          -- will NOT work as expected because of the ending comment -> */
/*
   cSpell:ignoreRegExp /q{5}/          -- will match qqqqq just fine but NOT QQQQQ
*/
// cSpell:ignoreRegExp /[^\s]{40,}/    -- will ignore long strings with no spaces.
// cSpell:ignoreRegExp Email           -- this will ignore email like patterns -- see Predefined RegExp expressions
var encodedImage = 'HR+cPzr7XGAOJNurPL0G8I2kU0UhKcqFssoKvFTR7z0T3VJfK37vS025uKroHfJ9nA6WWbHZ/ASn...';
var email1 = 'emailaddress@myfancynewcompany.com';
var email2 = '<emailaddress@myfancynewcompany.com>';
```

**Note:** ignoreRegExp and includeRegExp are applied to the entire file.  They do not start and stop.

#### Include Example

In general you should not need to use `includeRegExp`. But if you are mixing languages then it could come in helpful.

```Python
# cSpell:includeRegExp #.*
# cSpell:includeRegExp ("""|''')[^\1]*\1
# only comments and block strings will be checked for spelling.
def sum_it(self, seq):
    """This is checked for spelling"""
    variabele = 0
    alinea = 'this is not checked'
    for num in seq:
        # The local state of 'value' will be retained between iterations
        variabele += num
        yield variabele
```


## Predefined RegExp expressions

### Exclude patterns
* `Urls`<sup>1</sup> -- Matches urls
* `HexDigits` -- Matches hex digits: `/^x?[0-1a-f]+$/i`
* `HexValues` -- Matches common hex format like #aaa, 0xfeef, \\u0134
* `EscapeCharacters`<sup>1</sup> -- matches special characters: '\\n', '\\t' etc.
* `Base64`<sup>1</sup> -- matches base64 blocks of text longer than 40 characters.
* `Email` -- matches most email addresses.

### Include Patterns
* `Everything`<sup>1</sup> -- By default we match an entire document and remove the excludes.
* `string` -- This matches common string formats like '...', "...", and \`...\`
* `CStyleComment` -- These are C Style comments /* */ and //
* `PhpHereDoc` -- This matches PHPHereDoc strings.

<sup>1.</sup> These patterns are part of the default include/exclude list for every file.

## Customization

### Adding words to the Workspace Dictionary

You have the option to add you own words to the workspace dictionary.  The easiest,
is to put your cursor on the word you wish to add, hit `<leader>aw`  You will get a list
of suggestions and the option to add the word.

You can also type in a word you want to add to the dictionary: `:CocCommand cSpell.addWordToDictionary`
and type in the word you wish to add.

### cSpell.json

Words added to the dictionary are placed in the `cSpell.json` file in the `.vim` folder found in the _workspace_.
Note, the settings in cSpell.json will override the equivalent cSpell settings in settings.json.

#### Example _cSpell.json_ file
```javascript
// cSpell Settings
{
    // Version of the setting file.  Always 0.1
    "version": "0.1",
    // language - current active spelling language
    "language": "en",
    // words - list of words to be always considered correct
    "words": [
        "mkdirp",
        "tsmerge",
        "githubusercontent",
        "streetsidesoftware",
        "vsmarketplacebadge",
        "visualstudio"
    ],
    // flagWords - list of words to be always considered incorrect
    // This is useful for offensive words and common spelling errors.
    // For example "hte" should be "the"
    "flagWords": [
        "hte"
    ]
}
```

### Configuration Settings

```javascript
    //-------- Code Spell Checker Configuration --------
    // The Language local to use when spell checking. "en" and "en-GB" are currently supported.
    "cSpell.language": "en",

    // Controls the maximum number of spelling errors per document.
    "cSpell.maxNumberOfProblems": 100,

    // Controls the number of suggestions shown.
    "cSpell.numSuggestions": 8,

    // The minimum length of a word before checking it against a dictionary.
    "cSpell.minWordLength": 4,

    // Specify file types to spell check.
    "cSpell.enabledLanguageIds": [
        "csharp",
        "go",
        "javascript",
        "javascriptreact",
        "markdown",
        "php",
        "plaintext",
        "typescript",
        "typescriptreact",
        "yml"
    ],

    // Enable / Disable the spell checker.
    "cSpell.enabled": true,

    // Display the spell checker status on the status bar.
    "cSpell.showStatus": true,

    // Words to add to dictionary for a workspace.
    "cSpell.words": [],

    // Enable / Disable compound words like 'errormessage'
    "cSpell.allowCompoundWords": false,

    // Words to be ignored and not suggested.
    "cSpell.ignoreWords": ["behaviour"],

    // User words to add to dictionary.  Should only be in the user settings.
    "cSpell.userWords": [],

    // Specify paths/files to ignore.
    "cSpell.ignorePaths": [
        "node_modules",        // this will ignore anything the node_modules directory
        "**/node_modules",     // the same for this one
        "**/node_modules/**",  // the same for this one
        "node_modules/**",     // Doesn't currently work due to how the current working directory is determined.
        "vscode-extension",    //
        ".git",                // Ignore the .git directory
        "*.dll",               // Ignore all .dll files.
        "**/*.dll"             // Ignore all .dll files
    ],

    // flagWords - list of words to be always considered incorrect
    // This is useful for offensive words and common spelling errors.
    // For example "hte" should be "the"`
    "cSpell.flagWords": ["hte"],

    // Set the delay before spell checking the document. Default is 50.
    "cSpell.spellCheckDelayMs": 50,
```

## Dictionaries

The spell checker includes a set of default dictionaries.

### General Dictionaries

- **wordsEn** - Derived from Hunspell US English words.
- **wordsEnGb** - Derived from Hunspell GB English words.
- **companies** - List of well known companies
- **softwareTerms** - Software Terms and concepts like "coroutine", "debounce", "tree", etc.
- **misc** - Terms that do not belong in the other dictionaries.

### Programming Language Dictionaries

- **typescript** - keywords for Typescript and Javascript
- **node** - terms related to using nodejs.
- **php** - *php* keywords and library methods
- **go** - *go* keywords and library methods
- **python** - *python* keywords
- **powershell** - *powershell* keywords
- **html** - *html* related keywords
- **css** - *css*, *less*, and *scss* related keywords

### Miscellaneous Dictionaries

- **fonts** - long list of fonts - to assist with *css*

Based upon the programming language, different dictionaries will be loaded.

Here are the default rules: "*" matches any language.
`"local"` is used to filter based upon the `"cSpell.language"` setting.

``` javascript
{
"cSpell.languageSettings": [
    { "languageId": '*',      "local": 'en',               "dictionaries": ['wordsEn'] },
    { "languageId": '*',      "local": 'en-US',            "dictionaries": ['wordsEn'] },
    { "languageId": '*',      "local": 'en-GB',            "dictionaries": ['wordsEnGb'] },
    { "languageId": '*',                                   "dictionaries": ['companies', 'softwareTerms', 'misc'] },
    { "languageId": "python", "allowCompoundWords": true,  "dictionaries": ["python"]},
    { "languageId": "go",     "allowCompoundWords": true,  "dictionaries": ["go"] },
    { "languageId": "javascript",                          "dictionaries": ["typescript", "node"] },
    { "languageId": "javascriptreact",                     "dictionaries": ["typescript", "node"] },
    { "languageId": "typescript",                          "dictionaries": ["typescript", "node"] },
    { "languageId": "typescriptreact",                     "dictionaries": ["typescript", "node"] },
    { "languageId": "html",                                "dictionaries": ["html", "fonts", "typescript", "css"] },
    { "languageId": "php",                                 "dictionaries": ["php", "html", "fonts", "css", "typescript"] },
    { "languageId": "css",                                 "dictionaries": ["fonts", "css"] },
    { "languageId": "less",                                "dictionaries": ["fonts", "css"] },
    { "languageId": "scss",                                "dictionaries": ["fonts", "css"] },
];
}
```

### How to add your own Dictionaries

#### Global Dictionary

To add a global dictionary, you will need change your user settings.

##### Define the Dictionary

In your user settings, you will need to tell the spell checker where to find your word list.

Example adding medical terms, so words like *acanthopterygious* can be found.

```javascript
// A List of Dictionary Definitions.
"cSpell.dictionaryDefinitions": [
    { "name": "medicalTerms", "path": "/Users/guest/projects/cSpell-WordLists/dictionaries/medicalterms-en.txt"}
],
// List of dictionaries to use when checking files.
"cSpell.dictionaries": [
    "medicalTerms"
]
```

**Explained:** In this example, we have told the spell checker where to find the word list file.
Since it is in the user settings, we have to use absolute paths.

Once the dictionary is defined. We need to tell the spell checker when to use it.
Adding it to `cSpell.dictionaries` advises the spell checker to always include the medical terms when spell checking.

**Note:** Adding large dictionary files to be always used will slow down the generation of suggestions.

#### Project / Workspace Dictionary

To add a dictionary at the project level, it needs to be in the **cSpell.json** file.
This file can be either at the project root or in the .vim directory.

Example adding medical terms, where the terms are checked into the project and we only want to use it for .md files.
```javascript
{
    "dictionaryDefinitions": [
        { "name": "medicalTerms", "path": "./dictionaries/medicalterms-en.txt"},
        { "name": "cities", "path": "./dictionaries/cities.txt"}
    ],
    "dictionaries": [
        "cities"
    ],
    "languageSettings": [
        { "languageId": "markdown", "dictionaries": ["medicalTerms"] },
        { "languageId": "plaintext", "dictionaries": ["medicalTerms"] }
    ]
}
```

**Explained:** In this example, two dictionaries were defined: *cities* and *medicalTerms*.
The paths are relative to the location of the *cSpell.json* file.  This allows for dictionaries to be checked into the project.

The *cities* dictionary is used for every file type, because it was added to the list to *dictionaries*.
The *medicalTerms* dictionary is only used when editing *markdown* or *plaintext* files.

## FAQ

See: [FAQ](https://github.com/iamcco/coc-spell-checker/blob/master/FAQ.md)

### Buy Me A Coffee ☕️

![btc](https://img.shields.io/keybase/btc/iamcco.svg?style=popout-square)

![image](https://user-images.githubusercontent.com/5492542/42771079-962216b0-8958-11e8-81c0-520363ce1059.png)
