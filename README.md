# impl-package-metadata-index-json
A metadata implementation designed around a per-package (not per-dist) index of variants

The intent here is to provide a working implementation of [the scheme proposed by Oscar Benjamin](https://discuss.python.org/t/selecting-variant-wheels-according-to-a-semi-static-specification/53446).

That proposal incorporates a centralized metadata file describing available variants, like

```
# python-flint-0.6.0-wheel-selector.toml
[wheel-selector]

variables = ["x86_64_version"]

[selector.x86_64_version]

requires = ["cpuversion >= 1.0"]
function = ["cpuversion:get_x86_64_psABI_version"]

wheel_tags = {
    x86-64 = [""],
    x86-64-v2 = [""],
    x86-64-v3 = ["x86_64_v3", ""],
    x86-64-v4 = ["x86_64_v4", "x86_64_v3", ""],
}
```

This metadata captures the notion of a metadata provider tool with the spec:

```
requires = ["cpuversion >= 1.0"]
function = ["cpuversion:get_x86_64_psABI_version"]
```

Dists with variants are delineated with suffixes to the platform tag:

```
python_flint-0.6.0-cp312-cp312-win_amd64+x86_64_v3.whl
python_flint-0.6.0-cp312-cp312-win_amd64.whl
```
