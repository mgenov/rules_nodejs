load("@npm//webpack-dev-server:index.bzl", server = "webpack_dev_server")
load("@bazel_skylib//rules:write_file.bzl", "write_file")
load("@build_bazel_rules_nodejs//:index.bzl", "copy_to_bin")
# load("@npm//react-scripts:index.bzl", "react_scripts", "react_scripts_test")

copy_to_bin(
    name = "copy_static_files",
    srcs = glob(
        [
            "public/*",
            "src/**/*",
        ],
    ) + [
        "webpack.config.js",
        "tsconfig.json",
    ],
)

# react-scripts can only work if the working directory is the root of the application.
# So we'll need to chdir before it runs.
write_file(
    name = "write_chdir_script",
    out = "chdir.js",
    content = ["process.chdir(__dirname)"],
)

_RUNTIME_DEPS = [
    "chdir.js",
    "copy_static_files",
]

server(
    name = "server",
    args = [
        # "--node_options=--require=./$(execpath chdir.js)",
        "--hot",
        "--watch-poll",
        "--config ./$(execpath webpack.config.js)",
    ],
    data = _RUNTIME_DEPS + [
        "public/index.html",
        "src/index.tsx",
        "tsconfig.json",
        "webpack.config.js",
    ],
)
