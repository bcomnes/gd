# gv

A, maybe one day, go vendoring assistant.  Its a tool that **you** can use to help automate vendoring chores like:

- Vendor local dependencies into your module's `./vendor/` directory.
- Pin dependency versions at `git` tags and sha1 hashes.
- Experiment with what else git submodules allow.

**Goals:**

- Supplement the built in vendoring primitives provided by go.
- Metadata as an observation.  The source of truth is what go sees.  Use a metadata file only as a reflection of this truth.
- Don't stray away from existing go philosophies and conventions.
- Not like npm, not bundler.  But carry over some of the ux and concepts that work exceptionally well from existing tools.
- Avoid NIH.  Leverage the best vendoring description standard to date: `package.json`
- Take care of your own.  Don't dictate a vendoring strategy on dependencies deps, but be compatible with the default, unvendored strategy.
- Hide your vendoring strategy.  Don't dictate how consumers consume you.  This means `go get` should always work on `gv`'d packages.
- Keep it simple.  Say it again.

## Background

Go provides a unique set of primitives for vendoring code:

### ./vendor dir

```
If there is a source directory d/vendor, then, when compiling a source file within the subtree rooted at d, import "p" is interpreted as import "d/vendor/p" if that path names a directory containing at least one file with a name ending in “.go”.

When there are multiple possible resolutions, the most specific (longest) path wins.

The short form must always be used: no import path can contain “/vendor/” explicitly.

Import comments are ignored in vendored packages.
```

- https://docs.google.com/document/d/1Bz5-UB7g2uPBdOx-rw5t9MxJwkfpx90cqG9AFL0JAYo/edit

### go get properties

The `go get` command reads the import statements in a package, and then retrieves the HEAD of any remote repositories it finds.

Here are some other interesting properties:

- `go get` fetches a new dependency into $GOPATH.  It never places it in the vendor directory, so `gv` must handle this somehow.
- `go get` fetches submodules recursively.  This mechanism provides the only built in way to specify a particular version of a dependency through the use of pinned git submodules.
- Any missing packages end up in the global $GOPATH.  This isn't great, but its the default behavior.  Don't try to guard this.  This can only be solved through vendoring, so dependencies written to guard for this are the solution.

### Existing go conventions

- `go get` just works™
- Assume HEAD stable
- Utilize the stdlib
- Avoid deep dependency chains, keep it mostly flat

### Git Apparatus

- http://git-scm.com/book/en/v2/Git-Tools-Submodules
- http://git-scm.com/book/en/v1/Git-Tools-Subtree-Merging


### Community rationale

- https://nathany.com/go-packages/


### Existing attempts

- https://github.com/golang/go/wiki/PackageManagementTools
