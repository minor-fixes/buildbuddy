load("@bazel_gazelle//:def.bzl", "gazelle")
load("@io_bazel_rules_go//go:def.bzl", "nogo")
load("//tools:prettier.bzl", "prettier")

package(default_visibility = ["//visibility:public"])

nogo(
    name = "vet",
    vet = True,
    visibility = ["//visibility:public"],
)

# Ignore the node_modules dir
# gazelle:exclude node_modules
# Prefer generated BUILD files to be called BUILD over BUILD.bazel
# gazelle:build_file_name BUILD,BUILD.bazel
# gazelle:prefix github.com/buildbuddy-io/buildbuddy
# gazelle:proto disable
gazelle(name = "gazelle")

exports_files([
    "tsconfig.json",
    "package.json",
    "yarn.lock",
    "VERSION",
])

prettier(
    name = "check_prettier",
    mode = "check",
    patterns = [
        "app/**/*.*",
        "config/**/*.yaml",
    ],
)

filegroup(
    name = "config_files",
    srcs = select({
        ":release_build": ["config/buildbuddy.release.yaml"],
        "//conditions:default": glob(["config/**"]),
    }),
)

config_setting(
    name = "release_build",
    values = {"define": "release=true"},
)
