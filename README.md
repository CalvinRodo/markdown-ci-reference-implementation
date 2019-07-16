# markdown-ci-reference-implementation

A reference implementation for running CI on Markdown files.

---

## Table of Contents

---

- [markdown-ci-reference-implementation](#markdown-ci-reference-implementation)
  - [Table of Contents](#Table-of-Contents)
  - [Implementations](#Implementations)
    - [Travis](#Travis)
      - [Travis Build Status](#Travis-Build-Status)
      - [Travis Build Definition](#Travis-Build-Definition)
      - [Notes](#Notes)
    - [Azure DevOps](#Azure-DevOps)
      - [Azure DevOps Build Status](#Azure-DevOps-Build-Status)
      - [Azure DevOps Build Definition](#Azure-DevOps-Build-Definition)
  - [Static Code Analysis - Linting](#Static-Code-Analysis---Linting)
    - [Error Checking](#Error-Checking)
      - [Link to markdownlint tools](#Link-to-markdownlint-tools)
      - [Command to run markdownlint-cli](#Command-to-run-markdownlint-cli)
    - [Spell Checking](#Spell-Checking)
      - [Link to SpellCheck tools](#Link-to-SpellCheck-tools)
      - [Command to run cspell](#Command-to-run-cspell)
    - [Link Checker](#Link-Checker)
      - [Link to Link Checker Tool](#Link-to-Link-Checker-Tool)
      - [Command to run Link Checker](#Command-to-run-Link-Checker)
  - [Special Thanks](#Special-Thanks)

## Implementations

### Travis

#### Travis Build Status

[![Build Status](https://travis-ci.com/CalvinRodo/markdown-ci-reference-implementation.svg?branch=master)](https://travis-ci.com/CalvinRodo/markdown-ci-reference-implementation)

#### Travis Build Definition

[Link to .travis.yml](https://github.com/CalvinRodo/markdown-ci-reference-implementation/blob/master/.travis.yml)

#### Notes

We use the Node environment as not all of the tools used have docker images
provided through the node package manager.

**:exclamation: Please Note:** at this time in order to use yarn you must have a
`yarn.lock` file.
([See here for more](https://blog.travis-ci.com/2016-11-21-travis-ci-now-supports-yarn))

### Azure DevOps

#### Azure DevOps Build Status

[![Build Status](https://dev.azure.com/cicd-ref-imp/markdown-ci-reference-implementation/_apis/build/status/CalvinRodo.markdown-ci-reference-implementation?branchName=master)](https://dev.azure.com/cicd-ref-imp/markdown-ci-reference-implementation/_build/latest?definitionId=1&branchName=master)

#### Azure DevOps Build Definition

[Link to azure-pipelines.yml](https://github.com/CalvinRodo/markdown-ci-reference-implementation/blob/master/azure-pipelines.yml)

## Static Code Analysis - Linting

### Error Checking

We use the popular `markdownlint` library for linting our markdown files. We
will do this through the `markdownlint-cli` command line tool running in a
docker container for portability.

#### Link to markdownlint tools

- [Link to `markdownlint` GitHub page](https://github.com/DavidAnson/markdownlint)
- [Link to `markdownlint-cli` GitHub page](https://github.com/igorshubovych/markdownlint-cli)
- [Link to `markdownlint-cli` Docker image](https://hub.docker.com/r/06kellyjac/markdownlint-cli)

#### Command to run markdownlint-cli

> We are also leaving the default ruleset. These rules can be customized
> [see Instructions here.](https://github.com/igorshubovych/markdownlint-cli#configuration)

`docker run -v $PWD:/markdown 06kellyjac/markdownlint-cli -i node_modules **/*.md`

### Spell Checking

We use the `cspell` tool for checking the spelling in our markdown files.
There currently is no docker image for this tool and so must be installed
through npm or yarn.

#### Link to SpellCheck tools

- [Link to `cspell` GitHub page](https://github.com/streetsidesoftware/cspell)
- [Link to `cspell` npm page](https://www.npmjs.com/package/cspell)

#### Command to run cspell

> **Please note:** You *will* need to customize this command to do so
> [follow these instructions](https://www.npmjs.com/package/cspell)

`cspell **/*.md --color`

### Link Checker

We use the `markdown-link-check` to ensure that all of the links we are
referencing in our markdown file point to valid web sites. We are using
the docker image for this tool to ensure a consistent run environment.

#### Link to Link Checker Tool

- [Link to markdown-link-check GitHub page](https://github.com/tcort/markdown-link-check)
- [Link to markdown-link-check Docker Image](https://hub.docker.com/r/robertbeal/markdown-link-checker/)

#### Command to run Link Checker

> **Please Note:** The markdown link checker does not have functionality to
> exclude folders so we are using the find command to search for all markdown
> that aren't in the node_modules folder.  
> Due to the use of find this command is best run on posix compatible operating
> systems

```bash
find . -not -path './**/node_modules/*' -name "*.md" | \
xargs docker run --rm --read-only -v $PWD:/data robertbeal/markdown-link-checker
```

## Special Thanks

Thanks goes to the IT Strategy Team at ESDC. Most of this information is copied
from their work in automating their markdown strategy documentation.
