<img width="10%" src="./assets/logo.svg">

# neuron

[![AGPL](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://en.wikipedia.org/wiki/Affero_General_Public_License)
[![built with nix](https://img.shields.io/badge/Built_With-Nix-5277C3.svg?logo=nixos&labelColor=73C3D5)](https://builtwithnix.org)
[![Zulip chat](https://img.shields.io/badge/zulip-join_chat-brightgreen.svg)](https://funprog.zulipchat.com/#narrow/stream/231929-Neuron)

neuron is a system for managing your plain-text [Zettelkasten](https://neuron.zettel.page/2011401.html) notes. 

**Features**

- Extended Markdown for easy linking between zettels
- Web interface (auto generated static site)
- Graph view of zettels (organic category tree)
- CLI for creating new zettels with automatic ID

## Getting started

See [neuron.zettel.page](https://neuron.zettel.page/) for the full guide to installing and using neuron.

## Developing

When modifying `src`, use ghcid as instructed as follows to monitor compile errors:

```bash
nix-shell --run ghcid
```

You can test your changes by running it on the `./guide` (or any) zettelkasten as follows:

```bash
bin/run -d ./guide rib -wS
```

This command will also automatically recompile and restart when you change any of the Haskell source files.

### Running tests

Unit tests can be run via ghcid as follows:

```
bin/test
```

### Developing on reflex-dom-pandoc

Neuron delegates HTML rendering of the Pandoc AST to [reflex-dom-pandoc](https://github.com/srid/reflex-dom-pandoc). To hack on it, first [install Obelisk](https://github.com/obsidiansystems/obelisk#installing-obelisk) and then:

```sh
# This will clone the git repo of reflex-dom-pandoc at dep/reflex-dom-pandoc
ob thunk unpack dep/reflex-dom-pandoc

# Let's work on that repo
cd dep/reflex-dom-pandoc

# Run ghcid (using neuron's nix config)
nix-shell ../../shell.nix --run ghcid
```

Now as you edit the reflex-dom-pandoc sources, ghcid should give you compiler feedback. Once you are done with your changes, simply re-run neuron's ghcid or bin/run (see further above) and it should reflect your changes.

When you are done, commit your changes to reflex-dom-pandoc (presumably in a branch) and then `git push` it. Finally, you must "pack" the thunk and commit the changes to the neuron repo:

```sh
cd ../..  # Back to neuron
rm -rf dep/reflex-platform/dist-newstyle # cleanup build artifacts before packing
ob thunk pack dep/reflex-dom-pandoc
git add dep/reflex-dom-pandoc
```
