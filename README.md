# Arkouda (αρκούδα): NumPy-like arrays at massive scale backed by Chapel.
## _NOTE_: Arkouda is under the MIT license.

## Talks on Arkouda
[Mike Merrill's CHIUW 2019 talk](https://chapel-lang.org/CHIUW/2019/Merrill.pdf)

[Bill Reus' CLSAC 2019 talk](http://www.clsac.org/uploads/5/0/6/3/50633811/2019-reus-arkuda.pdf)


<<<<<<< HEAD
=======
To fill this gap, we have
developed a software package, called Arkouda, which allows a user to
interactively issue massively parallel computations on distributed
data using functions and syntax that mimic NumPy, the underlying
computational library used in the vast majority of Python data science
workflows. The computational heart of Arkouda is a Chapel interpreter
that
accepts a pre-defined set of commands from a client (currently
implemented in Python) and
uses Chapel's built-in machinery for multi-locale and multithreaded
execution. Arkouda has benefited greatly from Chapel's distinctive
features and has also helped guide the development of the language.

In early applications, users of Arkouda have tended to iterate rapidly
between multi-node execution with Arkouda and single-node analysis in
Python, relying on Arkouda to filter a large dataset down to a smaller
collection suitable for analysis in Python, and then feeding the results
back into Arkouda computations on the full dataset. This paradigm has
already proved very fruitful for EDA. Our goal is to enable users to
progress seamlessly from EDA to specialized algorithms by making Arkouda
an integration point for HPC implementations of expensive kernels like
FFTs, sparse linear algebra, and graph traversal. With Arkouda serving
the role of a shell, a data scientist could explore, prepare, and call
optimized HPC libraries on massive datasets, all within the same
interactive session.

## Requirements:
 * requires chapel 1.20.0 with the --legacy-classes flag
 * requires zeromq version >= 4.2.5, tested with 4.2.5 and 4.3.1
 * requires python 3.6 or greater
 * requires numpy
 * requires sphinx-doc to build python documentation
 
### It should be simple to get things going on a mac
```bash
brew install chapel
# you can also install python3 with brew
brew install python3
# the arkouda python client is available via pip
# pip will automatically install python dependencies (zmq and numpy)
# however, pip will not build the arkouda server (see below)
pip3 install arkouda
# these packages are nice but not a requirement
pip3 install pandas
pip3 install jupyter
```

### If you need to build Chapel from scratch here is what I use
```bash
# on my mac build chapel in my home directory with these settings...
# I don't understand them all but they seem to work
export CHPL_HOME=~/chapel/chapel-1.20.0
source $CHPL_HOME/util/setchplenv.bash
export CHPL_COMM=gasnet
export CHPL_COMM_SUBSTRATE=smp
export CHPL_GASNET_CFG_OPTIONS=--disable-ibv
export CHPL_TARGET_CPU=native
export GASNET_SPAWNFN=L
export GASNET_ROUTE_OUTPUT=0
export GASNET_QUIET=Y
export GASNET_MASTERIP=127.0.0.1
# Set these to help with oversubscription...
export QT_AFFINITY=no
export CHPL_QTHREAD_ENABLE_OVERSUBSCRIPTION=1
<<<<<<< HEAD
export CHPL_LLVM=llvm #the letters ll, not the numbers 11, l and 1 look similar in GitHub README.md
=======
>>>>>>> bac26cb8bce5b22a67872c723c6511a973a8ea29
cd $CHPL_HOME
make
```

## Building Arkouda

Download, clone, or fork the [arkouda repo](https://github.com/mhmerrill/arkouda). Further instructions assume that the current directory is the top-level directory of the repo.

If your environment requires non-system paths to find dependencies (e.g.,
if using the ZMQ and HDF5 bundled with [Anaconda]), append each path to a new file `Makefile.paths` like so:

```make
# Makefile.paths

# Custom Anaconda environment for Arkouda
$(eval $(call add-path,/home/user/anaconda3/envs/arkouda))
#                      ^ Note: No space after comma.
```

The `chpl` compiler will be executed with `-I`, `-L` and an `-rpath` to each
path.

Now, simply run `make` to build the `arkouda_server` executable.

[Anaconda]: https://www.anaconda.com/distribution/

## Building the Arkouda documentation
Make sure you installed the sphinx-doc package (e.g. `pip3 install -U Sphinx`)

Run `make doc` to build both the Arkouda python documentation and the Chapel server documentation

The output is currently in subdirectories of the `arkouda/doc`
```
arkouda/doc/python # python frontend documentation
arkouda/doc/server # chapel backend server documentation 
```

To view the documentation for the Arkouda python client, point your browser to `file:///path/to/arkouda/doc/python/index.html`, substituting the appropriate path for your configuration.

## Running arkouda_server

The command-line invocation depends on whether you built a single-locale version (with `CHPL_COMM=none`) or multi-locale version (with `CHPL_COMM` set).

Single-locale startup:

```bash
./arkouda_server
```

Multi-locale startup (user selects the number of locales):

```bash
./arkouda_server -nl 1
```

By default, the server listens on port `5555` and prints verbose output. These options can be changed with command-line flags `--ServerPort=1234` and `--v=false`.

## Testing arkouda_server

There is a small test program that connects to a running arkouda_server, runs a few computations, and shuts down the server. To run it, open a new terminal window in the arkouda directory and run

```bash
python3 tests/check.py localhost 5555
```

Substitute the correct hostname and port if you used a different configuration.

## Contributing to Arkouda

If you'd like to contribute, please see [CONTRIBUTING.md](CONTRIBUTING.md).
>>>>>>> master
