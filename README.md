# TreeSitterLanguages

Languages for the [Tree-sitter](https://tree-sitter.github.io/tree-sitter/) parser generator wrapped in Swift packages.

## Motivation

There are two reasons this package exists:

1. As an easy way to add several Tree-sitter languges to an app and particularly to an app that uses the [Runestone](https://github.com/simonbs/runestone) framework.
2. To show how a Tree-sitter language can be wrapped in a Swift package.

It is not recommended to use this package if you only ned to add a few Tree-sitter languages to your app. Instead you should add the languages manually.

## Installation

The languages are distributed using the [Swift Package Manager](https://www.swift.org/package-manager/) as separated libraries in a single Swift package. Install the package in your project by adding it as a dependency in your Package.swift manifest or through "Package Dependencies" in your project settings.

**NB:** Adding the package can take a a couple of minutes since it contains a lot of submodules that needs to be cloned.

```swift
let package = Package(
    dependencies: [
        .package(url: "git@github.com:simonbs/TreeSitterLanguages.git", from: "0.1.0")
    ]
)
```

## Usage

The Swift package contains multiple libraries. Each language is wrapped in its own Swift package so you don't need to import a whole suite of languages in case you only need a few.

The package contains the following threelibraries for each language.

|Name|Purpose|
|-|-|
|TreeSitter{Language}|The C code for the generated Tree-sitter parser. Each package exposes a function named like `tree_sitter_{language}`.|
|TreeSitter{Language}Queries|The queries to be used with the language. For more information on queries, please refer to [Tree-sitter's documentation](https://tree-sitter.github.io/tree-sitter/syntax-highlighting#queries). The only reason this is a separate library and not part of the TreeSitter{Language} library is that C code and Swift code cannot be mixed in a library using Swift Package Manager.|
|TreeSitter{Language}Runestone|Exposes the language to be used with [Runestone](https://github.com/simonbs/runestone). This library depends on the TreeSitter{Language} and TreeSitter{Language}Queries libraries and it is the only library you need to import when using Runestone. The library exposes the language as an extension on [TreeSitterLanguage](https://github.com/simonbs/Runestone/blob/main/Sources/Runestone/Language/TreeSitter/TreeSitterLanguage.swift) (e.g. `TreeSitterLanguage.javaScript`) that's ready to be used in Runestone. It also includes the indentation specification for select languages.|

Furthermore the package contains the TreeSitterLanguagesCommon library which exposes base types from Tree-sitter like `TSLanguage`. This is useful when you need to refer to these types in your source code, for example if you return a `TSLanguage` from a function.

## Compatibility

The parser.c file of a language is generated using [tree-sitter-cli](https://github.com/tree-sitter/tree-sitter/blob/master/cli/README.md) and a language generated with one version of the CLI may not be compatible with future versions of Tree-sitter. All languages in this repository are kept compatible with the version of Tree-sitter used by my [Runestone](https://github.com/simonbs/runestone) framework.

## Generate Languages

TreeSitterLanguage refers to the generated parsers for most languages. However, [tree-sitter-swift](https://github.com/alex-pinkus/tree-sitter-swift) does not include the parser (parser.c file) in the repository. That decision is [explained here](https://github.com/alex-pinkus/tree-sitter-swift#where-is-your-parserc). The `generate.sh` script in this repository can be run to generate and copy the parser.c file for the Swift language.

It's only necessary to run `generate.sh` when updating the version of tree-sitter-swift since the generated parser.c file is checked into this repository.
