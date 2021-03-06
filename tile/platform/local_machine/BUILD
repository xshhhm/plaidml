load("//bzl:plaidml.bzl", "plaidml_proto_library", "plaidml_cc_library", "plaidml_cc_test")

plaidml_proto_library(
    name = "proto",
    srcs = [
        "local_machine.proto",
    ],
    visibility = ["//visibility:public"],
    deps = [
        "//tile/proto:hal",
    ],
)

plaidml_cc_library(
    name = "local_machine",
    srcs = [
        "buffer.cc",
        "buffer.h",
        "copy_mem_strategy.cc",
        "copy_mem_strategy.h",
        "devinfo.h",
        "direct_mem_strategy.cc",
        "direct_mem_strategy.h",
        "factory.cc",
        "mem_cache.cc",
        "mem_cache.h",
        "mem_chunk.h",
        "mem_deps.cc",
        "mem_deps.h",
        "mem_strategy.h",
        "placer.h",
        "platform.cc",
        "platform.h",
        "program.cc",
        "program.h",
        "run_request.cc",
        "run_request.h",
        "shim.cc",
        "shim.h",
        "tmp_mem_strategy.cc",
        "tmp_mem_strategy.h",
    ],
    hdrs = [
        "local_machine.h",
    ],
    visibility = ["//visibility:public"],
    deps = [
        ":block_placer",
        ":loose_scheduler",
        ":proto_cc",
        ":tdep_scheduler",
        "//tile/base",
        "//tile/base:hal",
        "//tile/hal/util:selector",
        "//tile/hal/util:settings",
        "//tile/proto:hal_cc",
        "//tile/proto:support",
    ],
    alwayslink = 1,
)

plaidml_cc_test(
    name = "platform_test",
    srcs = ["platform_test.cc"],
    deps = [
        ":local_machine",
        "//base/config",
        "//plaidml:proto_cc",
        "//testing:plaidml_config",
        "//tile/base:platform_test",
        "//tile/hal/opencl",
    ],
)

plaidml_cc_library(
    name = "placer",
    hdrs = ["placer.h"],
    visibility = ["//visibility:private"],
    deps = [":scheduler"],
)

plaidml_cc_library(
    name = "block_placer",
    srcs = ["block_placer.cc"],
    hdrs = ["block_placer.h"],
    visibility = ["//visibility:private"],
    deps = [":placer"],
)

plaidml_cc_library(
    name = "naive_placer",
    srcs = ["naive_placer.cc"],
    hdrs = ["naive_placer.h"],
    visibility = ["//visibility:private"],
    deps = [":placer"],
)

plaidml_cc_library(
    name = "scheduler",
    srcs = [
        "schedule.cc",
        "scheduler.cc",
    ],
    hdrs = [
        "schedule.h",
        "scheduler.h",
    ],
    visibility = ["//visibility:private"],
    deps = [
        ":proto_cc",
        "//tile/lang",
    ],
)

plaidml_cc_library(
    name = "scheduler_test",
    testonly = True,
    srcs = [
        "scheduler_test.cc",
    ],
    hdrs = ["scheduler_test.h"],
    data = [
        "testdata/prng.tpb",
        "testdata/resnet50_train.tpb",
        "testdata/xception.tpb",
    ],
    visibility = ["//visibility:public"],
    deps = [
        ":block_placer",
        ":naive_placer",
        ":scheduler",
        "//base/util:runfiles_db",
        "//testing:gtest_main",
        "//testing:matchers",
        "//tile/proto:support",
    ],
    alwayslink = True,
)

plaidml_cc_library(
    name = "loose_scheduler",
    srcs = [
        "loose_scheduler.cc",
        "loose_scheduler.h",
    ],
    visibility = ["//visibility:private"],
    deps = [
        ":placer",
        ":scheduler",
    ],
)

plaidml_cc_test(
    name = "loose_scheduler_test",
    srcs = ["loose_scheduler_test.cc"],
    tags = ["rtest_fail"],
    deps = [
        ":loose_scheduler",
        ":scheduler_test",
    ],
)

plaidml_cc_library(
    name = "linear_scheduler",
    srcs = [
        "linear_scheduler.cc",
        "linear_scheduler.h",
    ],
    visibility = ["//visibility:private"],
    deps = [
        ":placer",
        ":scheduler",
    ],
)

plaidml_cc_test(
    name = "linear_scheduler_test",
    srcs = ["linear_scheduler_test.cc"],
    tags = ["rtest_fail"],
    deps = [
        ":linear_scheduler",
        ":scheduler_test",
    ],
)

plaidml_cc_library(
    name = "tdep_scheduler",
    srcs = [
        "tdep_scheduler.cc",
        "tdep_scheduler.h",
    ],
    visibility = ["//visibility:private"],
    deps = [
        ":placer",
        ":scheduler",
    ],
)

plaidml_cc_test(
    name = "tdep_scheduler_test",
    srcs = ["tdep_scheduler_test.cc"],
    tags = ["rtest_fail"],
    deps = [
        ":scheduler_test",
        ":tdep_scheduler",
    ],
)
