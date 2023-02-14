# Testing dependencies between Python projects

## Root folders:

```
.
├── pants.toml
└── src
    ├── client_code
    │   └── client_code
    │       ├── BUILD
    │       ├── __init__.py
    │       └── client.py
    └── ops_core
        └── ops_core
            ├── BUILD
            ├── __init__.py
            └── data_io
                ├── BUILD
                ├── __init__.py
                └── pathlib.py

7 directories, 8 files
```

```shell
❯ pants roots                                     
src
```

## Code

```python
# client.py
from ops_core.data_io.pathlib import AnyPath

def my_func():
    return AnyPath("a-folder")
```

```python
# pathlib.py
from pathlib import Path

def AnyPath(p):
    return Path(p)
```

## Outputs

```shell
❯ pants --no-pantsd dependencies --transitive src/client_code/client_code/client.py
14:21:31.74 [WARN] Pants cannot infer owners for the following imports in the target src/client_co
de/client_code/client.py:

  * ops_core.data_io.pathlib.AnyPath (line: 1)

If you do not expect an import to be inferrable, add `# pants: no-infer-dep` to the import line. O
therwise, see https://www.pantsbuild.org/v2.14/docs/troubleshooting#import-errors-and-missing-depe
ndencies for common problems.
```
