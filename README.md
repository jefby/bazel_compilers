# bazel_compilers

This repo hosts the bazel compiler defintions for my many cpp projects. I'm not a compiler expert, so use with caution. I'm trying to maintain a Raspberry Pi / Nvidia Jetson suite of compiler definitions in one repository, effectively mirroring the bazel crosstool [here](https://github.com/bazelbuild/bazel/tree/master/src/test/shell/bazel/testdata/bazel_toolchain_test_data)

Requires bazel version >= 0.25.1.

Supported compilers:

* aarch64: processor architecture used in Jetson TX1 TX2 Xavier and Nano products.

# usage

In your `WORKSPACE` file:

```
http_archive(
    name = "jefby_bazel_compilers",
    urls = [
      "https://github.com/jefby/bazel_compilers/archive/<commit hash>.tar.gz",
    ],
    strip_prefix = "bazel_compilers-< commit hash >",
)

load("@jefby_bazel_compilers//compilers:dependencies.bzl", "cross_compiler_dependencies")

cross_compiler_dependencies()
```

In your `.bazelrc` file:

```
build --compiler=compiler

build:aarch64 --crosstool_top=@murtis_bazel_compilers//compilers/arm_compiler:toolchain
build:aarch64 --host_crosstool_top=@bazel_tools//tools/cpp:toolchain
build:aarch64 --cpu=aarch64-linux-gnu --compiler=gcc
build:aarch64 --spawn_strategy=standalone
```


Build with:

```
# locally executable:
bazel build //example

# Nvidia Jetson Products:
bazel build --config=aarch64 //example
```
