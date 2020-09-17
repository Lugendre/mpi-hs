# Give your project a name. :)
workspace(name = "mpihs")

# Load the repository rule to download an http archive.
load(
    "@bazel_tools//tools/build_defs/repo:http.bzl",
    "http_archive"
)

# Download rules_haskell and make it accessible as "@rules_haskell".
http_archive(
    name = "rules_haskell",
    strip_prefix = "rules_haskell-0.12",
    urls = ["https://github.com/tweag/rules_haskell/archive/v0.12.tar.gz"],
    sha256 = "56a8e6337df8802f1e0e7d2b3d12d12d5d96c929c8daecccc5738a0f41d9c1e4",
)

load(
    "@rules_haskell//haskell:repositories.bzl",
    "rules_haskell_dependencies",
)

# Setup all Bazel dependencies required by rules_haskell.
rules_haskell_dependencies()

load(
    "@rules_haskell//haskell:toolchain.bzl",
    "rules_haskell_toolchains",
)

# Load nixpkgs_git_repository from rules_nixpkgs,
# which was already initialized by rules_haskell_dependencies above.
load(
    "@io_tweag_rules_nixpkgs//nixpkgs:nixpkgs.bzl",
    "nixpkgs_git_repository",
    "nixpkgs_cc_configure",
    "nixpkgs_python_configure",
    "nixpkgs_package",
)

nixpkgs_cc_configure(
    repository = "@nixpkgs",
)

nixpkgs_python_configure(
    repository = "@nixpkgs",
)

# Fetch a version of nixpkgs from GitHub.
# For more information see the documentation of rules_nixpkgs at
# https://github.com/tweag/rules_nixpkgs/blob/master/README.md
nixpkgs_git_repository(
    name = "nixpkgs",
    remote = "https://github.com/NixOS/nixpkgs",
    revision = "6d4b93323e7f78121f8d6db6c59f3889aa1dd931",
    sha256 = "56fe8d35597fc34bd63b667850933b6d96a9476964c5bcc958d844758c6bfa4c",
)

nixpkgs_package(
    name = "c2hs",
    repository = "@nixpkgs",
    attribute_path = "haskell.packages.ghc883.c2hs",
)

register_toolchains("//:c2hs")

nixpkgs_package(
    name = "openmpi",
    repository = "@nixpkgs",
)

load(
    "@rules_haskell//haskell:nixpkgs.bzl",
    "haskell_register_ghc_nixpkgs",
)

# Fetch a GHC binary distribution from nixpkgs and register it as a toolchain.
# For more information:
# https://api.haskell.build/haskell/nixpkgs.html#haskell_register_ghc_nixpkgs
haskell_register_ghc_nixpkgs(
    repository = "@nixpkgs",
    attribute_path = "haskell.compiler.ghc883",
    version = "8.8.3",
)

load(
    "@rules_haskell//haskell:cabal.bzl",
    "stack_snapshot",
)

stack_snapshot(
    name = "stackage",
    packages = [
        "base",
        "bytestring",
        "monad-loops",
    ],
    snapshot = "lts-15.8",
)
