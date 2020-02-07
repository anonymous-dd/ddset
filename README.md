# ddset

## Preparation

You need [fuzzingbook](https://pypi.org/project/fuzzingbook/) installed.
```shell
$ python3 -m pip install fuzzingbook
```

Extract all the archieve files. The following extracts all reproduction files to `Fabstract` directory and all results to `results`.

```shell
$ for i in *.gz; do gzip -dc $i | tar -xvpf -; done
```

The results directory has the following format:

* `(lang)_(bug).(langext).json` --- e.g. `clj-2092.clj.json`
  contains the output of reduction and abstraction. It can be viewed in a text file. `min_s` is the output of `perses` and
  `abs_s` is the compact representation while `abs_t` is the abstract derivation tree. The derivationtree can be viewed with
 
 ```shell
 $ python src/show_tree.py clj-2092.clj.json -c
 ```
Red color indicates abstract nodes.

* `(lang).(bug).(langext).log.json` --- e.g. `rhino.385.js.log.json`
  contains the log of executions required to produce abstractions

* `reduce_(lang)_bug.log`
  contains the log of reduction.
  
* `fuzz_(lang)_bug.log`
  contains the log of fuzzing. The last few lines in the log indicates how many total valid inputs were produced and how many
  reproduced the failure.

## Reproduction

Change directory to `Fabstract`.

```shell
$ cd Fabstract
```

Make each target (includes two steps -- reduction (which includes abstraction) and fuzzing)

```shell
$ make all_lua
$ make all_clojure
$ make all_rhino
$ make all_closure
```

To fuzz each target again, you can call the following (referred from *all* )

```shell
$ make fuzz_lua
$ make fuzz_clojure
$ make fuzz_rhino
$ make fuzz_closure
```

All results are stored in `results`. The execution results are cached in a `.db` directory

## Unix Utilities

The `find` and `grep` commands require `https://dbgbench.github.io/` to be installed, and the containers running. The container hashes are embedded in the scripts under `src`. These hashes need to be updated to the correct container id.
