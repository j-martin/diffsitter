# Contributing

## Building

This project uses a mostly standard Rust toolchain. At the time of writing, the
easiest way to get set up with the Rust toolchain is
[rustup](https://rustup.rs/). The rustup website has the instructions to get
you set up with Rust on any platform that Rust supports. This project uses
Cargo to build, like most Rust projects.

The only small caveat with this projects that isn't standard is that it has
bindings to tree-sitter, so it compiles tree-sitter grammars that are written
in C and C++, and uses the C FFI to link from the Rust codebase to the
compiled tree-sitter grammars. As such, you'll need to have a C compiler that
supports `C99` or later, and a C++ compiler that supports `C++14` or later.
Compilation is handled by the `cc` crate, and you can find the details on how
compilers are selected in the [cc docs](https://docs.rs/cc).

These tree-sitter grammars are included as [git
submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), so you need
to make sure you initialize submodules when checking out the git repo.

If you're cloning the repository for the first time:

```sh
git clone --recurse-submodules https://github.com/afnanenayet/diffsitter.git
```

If you've already checked out the repository, you can initialize submodules
with the following command:

```sh
git submodule update --init --recursive
```

This command can also be used to update to the latest commits for each
submodule as the repository gets updated. Sometimes you may run into build
errors that complain about trying to link to nonexistent symbols, this error
can be incurred if a new grammar is added to the repository but the source
files aren't present, so you should run the update command to see if that fixes
the error. If it doesn't, I've messed up and you should file an issue
(with as much detail as possible).

### C/C++ Toolchains

If you're on Mac and have [Homebrew](https://brew.sh) installed:

```sh
brew install llvm

# or

brew install gcc
```

The built-in Apple clang that comes with XCode is also fine.

If you're on Ubuntu:

```sh
sudo apt install gcc
```

If you're on Arch Linux:

```sh
sudo pacman -S gcc
```

## Development

There's not much to say about the architecture at the moment, this is a
relatively small codebase and subject to change as we receive more feedback. I
try to keep the codebase well-commented and easy to follow, but feel free to
file issues about confusing architectural decisions or incomplete/underwhelming
documentation.

If you want to contribute, you need to make sure that the project builds and
that tests pass, which you can check locally with:

```sh
cargo test --all
```

The CI will test the project on all major OS's and some additional platforms on
Linux, such as ARM (using the `cross` toolchain). Having these checks all pass
is a prerequisite for getting any PR merged. I've found that tests can be a
little flaky on the Windows platform check, so if you see that tests failed
there, try re-running the checks with Github actions to see if they pass.

This project targets the latest stable version of `rustc`.