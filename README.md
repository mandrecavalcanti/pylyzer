# pylyzer ⚡

<a href="https://marketplace.visualstudio.com/items?itemName=pylyzer.pylyzer" target="_blank" rel="noreferrer noopener nofollow"><img src="https://img.shields.io/visual-studio-marketplace/v/pylyzer.pylyzer?style=flat&amp;label=VS%20Marketplace&amp;logo=visual-studio-code" alt="vsm-version"></a>
<a href="https://github.com/mtshiba/pylyzer/releases"><img alt="Build status" src="https://img.shields.io/github/v/release/mtshiba/pylyzer.svg"></a>
<a href="https://github.com/mtshiba/pylyzer/actions/workflows/rust.yml"><img alt="Build status" src="https://github.com/mtshiba/pylyzer/actions/workflows/rust.yml/badge.svg"></a>

`pylyzer` is a static code analyzer / language server for Python, written in Rust.

## Installation

### cargo (rust package manager)

```bash
cargo install pylyzer
```

### pip

```bash
pip install pylyzer
```

If installed this way, you also need to [install Erg](https://github.com/mtshiba/ergup).

```bash
curl -L https://github.com/mtshiba/ergup/raw/main/ergup.py | python3
```

## What is the advantage over pylint, pyright, pytype, etc.?

* Performance 🌟

On average, pylyzer can inspect Python scripts more than __100 times faster__ than pytype and pyright [<sup id="f1">1</sup>](#1). This is largely due to the fact that pylyzer is implemented in Rust.

![performance](https://raw.githubusercontent.com/mtshiba/pylyzer/main/images/performance.png)

* Detailed analysis 🩺

pylyzer can do more than the type checking. For example, it can detect out-of-bounds accesses to lists and accesses to nonexistent keys in dicts.

![analysis](https://raw.githubusercontent.com/mtshiba/pylyzer/main/images/analysis.png)

* Reports readability 📖

While pytype/pyright's error reports are illegible, pylyzer shows where the error occurred and provides clear error messages.

### pylyzer 😃

![report](https://raw.githubusercontent.com/mtshiba/pylyzer/main/images/report.png)

### pyright 🙃

![pyright_report](https://raw.githubusercontent.com/mtshiba/pylyzer/main/images/pyright_report.png)

* Rich LSP support 📝

pylyzer as a language server supports various features, such as completion and renaming (The language server is an adaptation of the Erg Language Server (ELS). For more information on the implemented features, please see [here](https://github.com/erg-lang/erg/tree/main/crates/els#readme)).

![lsp_support](https://raw.githubusercontent.com/mtshiba/pylyzer/main/images/lsp_support.png)

## How it works

pylyzer uses the type checker of [the Erg programming language](https://erg-lang.org) internally.
This language is a transpiled language that targets Python, and has a static type system.

pylyzer converts Python ASTs to Erg ASTs and passes them to Erg's type checker. It then displays the results with appropriate modifications.

## Limitations

* pylyzer's type inspector only assumes (potentially) statically typed code, so you cannot check any code uses reflections, such as `exec`, `setattr`, etc.

* pylyzer (= Erg's type system) has its own type declarations for the Python standard APIs. Typing of all APIs is not complete and may result in an error that such an API does not exist.

## TODOs

* [x] type checking
  * [x] variable
  * [x] operator
  * [x] function/method
  * [x] class
* [x] type inferring
  * [x] variable
  * [x] operator
  * [x] function/method
  * [x] class
* [x] builtin modules resolving (partially)
* [x] local scripts resolving
* [ ] local packages resolving
* [ ] compound type checking
  * [x] `Union`
  * [x] `Optional`
  * [x] `list`
  * [ ] others
* [ ] type variable (`TypeVar`, `Generic`)
* [ ] type assertion (`typing.cast`)

---

<span id="1" style="font-size:x-small"><sup>1</sup> The performance test was conducted on MacBook (Early 2016) with 1.1 GHz Intel Core m3 processor and 8 GB 1867 MHz LPDDR3 memory.[↩](#f1)</span>
