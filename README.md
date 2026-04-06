# Py2Strap

> pps is pypy bootstrapper that uses Python tricks to aim to simplify setting up and running code.
> because coding should be fun AND fast.

Quick start:

```shell
./setup latest                  # pypy + pip, no project strucs
./setup latest myapp            # + project structure (src/utils)
./setup latest myapp lint       # + ruff + pyproject.toml
./setup v7.3.19 myapp lint      # specific pypy version

# for the purists:
BARE=1 ./setup latest test       # bare pypy, no pip, with strucs, no linting
```

## Why pps

### Built-ins

> No imports needed.

```python
debug("verbose")      # hidden unless DEBUG=1
info("normal")        # colored: green/yellow/red
warn("careful")
error("bad")

@typed
def add(a: int, b: int) -> int:
    return a + b
```

### Explicit imports

> No forced `.py` ext, no packages/modules or relative imports, or `__init__.py` annoying files.

```python
greet = load_mod("utils/greet")
# use a def of module
greet.say("World")
# or single/multiple functions
inverse, say = load_def("utils/greet", "inverse", "say")
say(inverse("World"))
## we hit a cache here so not extra work
## altho might have already been imported.
# or run a mod directly
load_run("utils/greet")
# still avoid loads in hot-loops
```

### All flags

```shell
# run is symlinked originally to the extract .pypy{version}/bin/pypy3.11

./run pps [path]            # run a script
./run pps -p                # show pypy path
./run pps -v                # show pypy version
./run pps -i pkg1 pkg2 ...  # install packages (with dep tracking)
./run pps -F requirements   # install from requirements File
./run pps -u pkg1 pkg2 ...  # uninstall packages (with dep cleanup)
./run pps -l [path]         # lint with ruff (defaults to .)
./run pps -f [path]         # format and fix with ruff 
./run pps -r                # nuke pypy installation (with confirmation)

./run --jit [opts] pps      # JIT tuning (defaults are already optimal):
# threshold=200             compile sooner (default 1039)
# decay=0                   no counter decay (long-running servers)
# trace_limit=20000         longer traces before abort (default 6000)
```

> JIT defaults are well-tuned out of the box. Only tweak for niche cases.
> See [JIT help](https://doc.pypy.org/en/latest/jit_help.html) and [Performance](https://pypy.org/performance.html).

You can also pass regular python flags which translate just fine.
