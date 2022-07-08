# Introduction

TODO: Give a short introduction of your project. Let this section explain the objectives or the motivation behind this project.

## Getting Started

TODO: Guide users through getting your code up and running on their own system. In this section you can talk about:

1. Installation process
    1. Install [Visual Studio Code](https://code.visualstudio.com/download)
    1. Install [Git](https://git-scm.com/download/win)
       Select Use the native Windows Secure Channel library
    1. Install PowerShell extension
        Ctrl-Shift-P
        Extensions: Install Extensions
        Search PowerShell (PowerShell Preview extension is probably a better choice)
        Click Install
        Click Reload
    1. Clone Git
        Click Clone Git repository with VS Code
            A. Ctrl-Shift-P
            B. git: Clone
            C. Url: [https://dev.azure.com/uw-microsoft-infrastructure/PowerShell/_git/MyRepo](https://dev.azure.com/uw-microsoft-infrastructure/PowerShell/_git/MyRepo) (Or desired repository)
        D. Parent Directory: C:\users\MyUsername (Your UWNetid if working at UW)
            Alternatively use Git Gui
    1. Install VSCode Extensions (If using VSCode as you editor)
        A. Project Snippets by Peng Lv
            Select Snippets (Ctrl-Shift-x)
            Search for 'Project Snippets'
            Click Install for 'Project Snippets' by Peng Lv
        B. markdownlint by David Anson
            Select Snippets (Ctrl-Shift-x)
            Search for 'markdownlint'
            Click Install for 'markdownlint' by David Anson
1.	Software dependencies
1.	Latest releases
1.	API references

## Project Principles, Style and Conventions

### Single Responsibility

- Keeps functions and modules simple and specific
- Think, "Do one thing and do it well."
- Single function per file means less scrolling and searching
- Supports composability
- Make unit testing specific and effective

### DAMP

- Direct and Meaningful Phrases (DAMP) and DRY code i.e. long variable and function names, not the DRY vs. WET argument, and not DAMP unit testing (see below)
- DRY is ok if in-house code can't be depended upon
- No comments, use Write-Verbose to describe what is happening
    - An exception is explaining RexEx and complex math
    - i.e. Put examples of what RexEx should match in a comment
    - i.e. Explain unusual math computations in comment, e.g. what a modulus operation is trying to do
- No printf style output, confusing for other script developers
- Prefer json over xml
- Use full CmdLet names, not alias shortcuts, i.e.
    - Use Get-ChildItem, avoid gci, dir, or ls
    - Use Where-Object, avoid ?
    - Use ForeEach-Object, avoid %

### PowerShell Language Conventions

- __Must__ support pipelining
- Use common naming between modules, i.e. -ComputerName in all modules
- Aliases __only__ to support differences between external APIs, e.g. Name in one API and ComputerName another
    - Support native property name as default and other APIs as an alias (Example between Event CI and AD Computer)
- Emit only objects to output (I'm dubious on this now, due to the next point, emit one object at a time.)
    - Write-Output only in the End{} block
    - Write-Output should not be called anywhere else
- Emit one object at a time
    - i.e. Don't collect objects in an array then Write-Output said array
- Use approved PowerShell verbs
- Only use parameter position 0 for the most common function parameter
    - Parameter 0 is often -ComputerName
    - All other parameters are required to be named
    - Positional parameters are error prone and difficult read/debug later

### Unit Testing

- All functions must have 1 or more unit tests
- Use WET/DAMP unit test, i.e. Write Everything Twice, or We Enjoy Typing
   - This avoids writing code for unit tests and having to unit test the unit tests
   - Duplication is OK in tests.
- Minimum should test examples in the help documents
- Any bugs  __must__ be added as a unit test before fixing

## Project Structure

### Root files

- .gitignore
    - Ignore Pester test results
    - Ignore compiled documentation
- ReadMe.md
    - This document, living document that may be updated at any time and as desired
- Invoke-Documentation.ps1
    - Depends upon the PowerShell module PlatyPS (Install-Module -Name PlatyPS)
    - Creates the documentation skeleton MarkDownFiles (see Documentation below)
    - Compiles documentation into external, MAML help file for published module
- Invoke-Tests.ps1
    - Depends upon the PowerShell module Pester, shipped with Server 2016+, (Install-Module -Name Pester)
    - Invokes and runs unit tests (see Tests below)
    - Publishes results in nUnit format
    - Publishes code coverage in JaCoCo format

### Documentation

- In the .\docs directory
- Documentation is separate from code but compiled to module
    - Update code without risk of breaking code
    - Documentation lives with code
- Allows publishing to updatable help site, __again__
    - Update code without risk of breaking code
    - Documentation lives with code
- Written explicitly in MarkDown
    - Compiled into MAML
    - Presented as HTML in AZ DevOps, GitHub, other Git repositories, VSCode
    - Build converters exist
    - Common format
- Process:
    - Run Invoke-Documentation
    - Fill in synopsis
    - Fill in description
    - Fill in parameter descriptions
    - Write and describe all examples, to be used as tests (see Test below)

### Help

- This directory is empty unless Invoke-Documentation is called
- It's contents may exist locally after compiling documentation
- The contents are created by the build server and included in the published module
- The compiled MAML is included in .\<Module Name>\en-US

### Tests

- .\<module name>.tests.ps1
    - This should be the only module in the root of .\Tests
    - This will test the module functionality, e.g. does the module load without error
- In the .\Tests directory
- Contains all tests
- The .\Tests\Functions directory __exactly__ mirrors the <Module Name> directory
    - i.e. Each file in teh .\<Module Name> directory has a <filename>.Tests.ps1 file in .\Tests\Functions
- The .\Tests\Artifacts directory contains files required for unit tests which may be any of the following:
    - Pester Mock parameters
    - Pester MockWith Return data
    - MockServer Query String or Post data
    - MockServer Return data
    - Other test data and artifacts, e.g. data.csv to be available in Test:\ drive

### Module Directory

- Contains functional code
- <module name>.psd1
    - Set the version using [Semantic Versioning](https://semver.org/)
    - Defines exported Functions, Variables, Aliases, etc.
- <module name>.psm1
    - Dot sources all functions, exported or not
    - Excludes *tests.ps1 to avoid testing on customer machines
    - Calls Initialize-ModuleConfiguration to make sure config file exists
- .\<module name>\Functions
    - Contains all executable code
    - No separate "private", "public" folders, managed in manifest and easier to find/understand functions in the module context
    - Do not use export member, export don in the manifest, i.e. only one place to look for what is public
    - One function, one file, i.e. keeps with single responsibility principle and easier to find, i.e. less scrolling and searching
    - No __contextual help__, use MarkDown and PlatyPS in .\docs instead
        - less scrolling and searching in code file
        - documentation is separate from code, i.e. documentation can be updated without risk of breaking code
- .\<module name>\bin
    - Executables and dlls required to support or run the module
- .\<module name>\library
    - Any files other that executables and dlls required to support or run the module
    - e.g. Network data module

# Build and Test
TODO: Describe and show how to build your code and run the tests.

# Contribute
TODO: Explain how other users and developers can contribute to make your code better.

If you want to learn more about creating good readme files then refer the following [guidelines](https://www.visualstudio.com/en-us/docs/git/create-a-readme). You can also seek inspiration from the below readme files:
- [ASP.NET Core](https://github.com/aspnet/Home)
- [Visual Studio Code](https://github.com/Microsoft/vscode)
- [Chakra Core](https://github.com/Microsoft/ChakraCore)