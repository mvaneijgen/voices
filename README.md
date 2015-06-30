[![npm version](https://img.shields.io/npm/v/voices.svg)](https://npmjs.com/package/voices) [![license](https://img.shields.io/npm/l/voices.svg)](https://github.com/mklement0/voices/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [voices &mdash; introduction](#voices-&mdash-introduction)
- [Examples](#examples)
- [Installation](#installation)
  - [From the npm registry](#from-the-npm-registry)
  - [Manual installation](#manual-installation)
- [Usage](#usage)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# voices &mdash; introduction

`voices` is an OSX CLI for changing the default TTS (text-to-speech) and VoiceOver voice and for printing information about and speaking text with multiple voices.

`voices` complements the standard `say` utility by:

 * providing a way to change the default voice
 * operating on _active_ voices - those selected for active use - rather than all _installed_ voices
 * filtering voices by language
 * speaking text with _multiple_ voices.

_Caveat_: OSX, as of OSX 10.10, offers no documented programmatic way to change the default voice. Thus, this utility makes use of undocumented system internals, which, unfortunately, means that future compatibility cannot be guaranteed.

# Examples

```shell
  # List all active voices; add -a to list all installed ones.
voices -l

  # Print information about the default voice and speak its demo text.
voices -d -k

  # Print information about voice 'Alex'.
voices alex

  # Make 'Alex' the new default voice, print information about it, and
  # speak text that announces the change.
voices -k'The new default voice is Alex.' -d alex

  # List languages for which at least one voice is active.
voices -L

  # List active French voices.
voices -l fr

  # Print information about all active voices and speak the
  # their respective demo text.
voices -l -k

  # Say "hello", first with Alex, then with Jill, suppressing printed
  # output.
voices -k"hello" -q alex jill

  # Print information about all active Spanish voices and speak their
  # respective demo text.
voices -k -l es
```

# Installation

**Supported platforms**

* **OSX**

Tested on OS X 10.10, expected to work on 10.8+, but, due to use of undocumented system internals, future compatiblity cannot be guaranteed.

## From the npm registry

With [Node.js](http://nodejs.org/) or [io.js](https://iojs.org/) installed, install [the package](https://www.npmjs.com/package/voices) as follows:

    [sudo] npm install voices -g

**Note**:

* Whether you need `sudo` depends on how you installed Node.js / io.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* The `-g` ensures [_global_ installation](https://docs.npmjs.com/getting-started/installing-npm-packages-globally) and is needed to put `voices` in your system's `$PATH`.

## Manual installation

* Download [the CLI](https://raw.githubusercontent.com/mklement0/voices/stable/bin/voices) as `voices`.
* Make it executable with `chmod +x voices`.
* Move it or symlink it to a folder in your `$PATH`, such as `/usr/local/bin`.

# Usage

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```
$ voices --help

SYNOPSIS
  Get or set or speak with the default voice:
    voices [-k[-|"text"]] [options] [-d [newDefaultVoice]]
  List information about / speak with voices:
    voices [-k[-|"text"]] [options] voice...
  List / speak with all voices, optionally filtered by languages:
    voices [-k[-|"text"]] [options] -l [lang...]
  List languages among voices:
    voices [-a] -L
  Manage voices in System Preferences:
    voices -m

DESCRIPTION
  Sets the default voice for OS X's text-to-speech synthesis and voice-over or 
  returns information about the default, active and installed voices.
  Optionally speaks demo or specifiable text with all targeted voices.

  Case doesn't matter when specifying voice or language names.
   - Specify voice names as they appear in System Preferences > 
     Dictation & Speeech and in the output from `say -v \?`.
   - Specify languages as two-character language IDs (e.g., 'en'), optionally
     followed by '_' and a region identifier full (e.g., 'en_US').

  -l and -L target all *active* voices by default, which are typically a 
  a *subset* of all *installed* voices, and constitute the set of voices for
  active use as selected in System Preferences > Dictation & Speech >
  Text to Speech; adding -a targets all installed voices.

  The -k option for speaking with all targeted voices as well as other 
  shared options are discussed further below. Without -k, only printed output
  is produced; conversely, -q silences printed output.

  1st synopsis form: -d [newDefaultVoice]
    Returns information about the default voice or sets a new default voice.

    Note that any installed voice can be specified as the default voice, not
    just a currently active one.

  2nd synopsis form: voice...
    Lists information about the specified voices (whether active or not).

  3rd synopsis form: -l [lang...]
    Lists information about active, installed, or voices matching one or more
    specified languages.

    Lists all active voices by default;
    -a lists all installed ones instead.

    If at least one <lang> operand is given, the list of active voices (by
    default) / installed voices (with -a) is filtered to output only those
    matching the specified language(s).
    
    <lang> values may be mere language IDs (e.g., 'en') or language + region
    IDs (e.g., 'en_US'); e.g., 'en' matches all English voice irrespective of
    region, whereas 'en_US' matches only US English voices.

  4th synopsis form: -L
    Lists the distinct set of languages supported among all active (by default)
    or all installed (-a) voices.
    
    Languages are listed as language + region identifiers, e.g., 'en_US'.

  5th synopsis form:
    Opens System Preferences > Dictation & Speech, where you can manage the
    set of active voices, install additional voices, and control other aspects
    of text-to-speech synthesis.

  Shared options:

    -q
      Quiet mode: suppresses printed output, such as when only speech output
      (-k) is desired or when the new default voice should be set quietly.

    Speaking options (synopsis forms 1-3): -k
      Note that if the command targets multiple voices, speaking happens
      after each voice's information has been printed, unless suppressed with
      -q.

      -k (no argument)
        Speaks each voice's demo text.
      -k"text"
        Speaks the specified text using each voice.
        Note that "text" must be directly attached to -k and must be quoted
        to protect it from (unwanted) interpretation by the shell.
      -k-
        Speaks text provided via stdin using each voice.

    Printed-output options (synopsis forms 1-3): -b or -i
      By default, voice information is in the form provided by the standard
      `say` utility when invoked as `say -v \?`, which is:
        <voice> <lang> # <demo text>
      The following, mutually exclusive options modify this behavior:

      -b
        Outputs mere voice names only.
      -i
        Outputs internal voice identifiers, as used by the system.

NOTES
  Tested on OS X 10.10, expected to work on 10.8+, but due to use of
  undocumented system internals future compatiblity cannot be guaranteed.

  The focus of this utility is speaking text with *multiple* voices, changing
  the default voice, and browsing active voices, optionally filtered by 
  language.
  By contrast, to just speak text with the default voice or a specified single
  voice, it is easier to use `say` directly; e.g.:
    say -v Alex I am. # equivalent of: voices -k"I am." Alex

EXAMPLES
    # List all active voices; add -a to list all installed ones.
  voices -l         
    # Print information about the default voice and speak its demo text.
  voices -d -k
    # Print information about voice 'Alex'.
  voices alex
    # Make 'Alex' the new default voice, print information about it, and 
    # speak text that announces the change. 
  voices -k'The new default voice is Alex.' -d alex 
    # List languages for which at least one voice is active.
  voices -L
    # List active French voices.
  voices -l fr
    # Speak the respective demo text with all active voices.
  voices -l -k
    # Speak "hello" first with Alex, then with Jill, suppressing printed
    # output.
  voices -k"hello" -q alex jill
    # Print information about all active Spanish voices and speak their
    # respective demo text.
  voices -k -l es
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2015 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have optional suffixes denoting the type of dependency; the *absence* of a suffix denotes a required *run-time* dependency: `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [replace (D)](https://github.com/harthur/replace)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [tap (D)](https://github.com/isaacs/node-tap)
* [urchin (D)](https://github.com/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **v0.1.0** (2015-06-29):
  * Initial release.
