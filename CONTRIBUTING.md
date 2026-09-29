# Contributing to Pyrefly

Welcome! We’re excited that you’re interested in contributing to Pyrefly. Whether you’re fixing a bug, adding a feature, or improving documentation, your help makes Pyrefly better for everyone.

## Getting Started

The [rust toolchain](https://www.rust-lang.org/tools/install) is required for
development. You can use the normal `cargo` commands (e.g. `cargo build`,
`cargo test`).

## Choosing what to work on

We ask that contributors please look at our open GitHub issues for tasks to work on and discuss approaches with maintainers, rather than going straight to opening a PR.

When looking for an issue to pick up, consider the following things:

1. it has a [good first issue](https://github.com/facebook/pyrefly/issues?q=is%3Aissue%20state%3Aopen%20label%3A%22good%20first%20issue%22) or [help wanted](https://github.com/facebook/pyrefly/issues?q=is%3Aissue%20state%3Aopen%20label%3A%22help%20wanted%22) label
2. it's not already assigned to anyone, or is assigned to someone but appears abandoned
3. there aren't any open PRs for it (or there are open PRs but they look stale/abandoned)
4. the issue still reproduces in the sandbox, or locally on a build from the main branch
5. the issue is part of an upcoming milestone - these are the highest priority issues to focus on
6. the issue does not have the "needs discussion" tag - typically issues with that tag don't have a clear solution that everyone agrees on yet so they are not "shovel ready", but feel free to participate in the discussion!
7. when you find an issue you want to pick up, comment `#claim` on it to self-assign (see [Repository automation](#repository-automation) below).

## Repository automation

GitHub bots help manage issues and pull requests.

### Claiming issues: `#claim` / `#unclaim`

To pick up an issue, comment `#claim` on it and the bot will assign it to you. When you're done — or if you decide not to work on it after all — comment `#unclaim` to release it so someone else can take over.

- `#claim` only works on **unassigned** issues. If the issue is already claimed by someone else, the bot leaves the existing assignee in place and tells you to coordinate with them — it won't reassign the issue to you. If it's already assigned to you, it just confirms that.
- `#unclaim` only removes *your own* assignment, and only if you're currently assigned.
- Both commands are case-insensitive and can appear anywhere in a comment (e.g. "I'd like to work on this, #claim").
- If the bot can't assign you automatically (GitHub only allows assigning users with repository access), it leaves a comment so a maintainer can assign you manually.

## Developing Pyrefly

Development docs are WIP. Please reach out if you are working on an issue and
have questions or want a code pointer.

As described in the
[architecture overview](https://github.com/facebook/pyrefly/blob/main/ARCHITECTURE.md),
our architecture follows 3 phases:

1. figuring out exports
2. making bindings
3. solving the bindings

Here's an overview of some important directories:

- `pyrefly/lib/alt` - Solving step
- `pyrefly/lib/binding` - Binding step
- `pyrefly/lib/commands` - Pyrefly startup
- `pyrefly/lib/error` - How we collect and emit errors
- `pyrefly/lib/export` - Exports step
- `pyrefly/lib/lsp` - Language server protocol (LSP) functionality
- `pyrefly/lib/module` - Import resolution/module finding logic
- `pyrefly/lib/solver` - Solving type variables and checking if a type is
  assignable to another type
- `pyrefly/lib/state` - Internal state for the language server
- `pyrefly/lib/test` - Integration tests for the typechecker
- `pyrefly/lib/test/lsp` - Integration tests for the language server
- `conformance` - Typing conformance tests pulled from
  [python/typing](https://github.com/python/typing/tree/main/conformance). Don't
  edit these manually. Instead, run `test.py` and include any generated changes
  with your PR.
- `crates/pyrefly_build` - (experimental) Build system support
- `crates/pyrefly_bundled` - Bundled typeshed and popular third party package stubs
- `crates/pyrefly_config` - Pyrefly configuration
- `pyrefly_derive` - Utility Rust macros
- `crates/pyrefly_python` - Utilities around Python functionality that are reusable across Pyrefly
- `crates/pyrefly_types` - Pyrefly internal representation of types
- `crates/pyrefly_util` - General utilities that are reused across Pyrefly
- `crates/tsp_types` - Utilities for type server protocol (TSP) functionality
- `test` - Markdown end-to-end tests for CLI features
- `website` - Source code for [pyrefly.org](https://pyrefly.org)

## Packaging

We use [maturin](https://github.com/PyO3/maturin) to build wheels and source
distributions. This also means that you can pip install `maturin` and, from the
inner `pyrefly` directory, use `maturin build` and `maturin develop` for local
development. `pip install .` in the inner `pyrefly` directory works as well. You
can also run `maturin` from the repo root by adding `-m pyrefly/Cargo.toml` to
the command line.

## Coding conventions

We follow the
[Buck2 coding convention](https://github.com/facebook/buck2/blob/main/docs/developers/basics.md),
with the caveat that we use our internal error framework for errors reported by
the type checker.

## Testing

You can use `cargo test` to run the tests, or `python3 test.py` from this
directory to use our all-in-one test script that auto-formats your code, runs
the tests, and updates the conformance test results. It requires Python 3.9+.

Here's where you can add new integration tests, based on the type of issue
you're working on:
