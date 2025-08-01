# Copyright (c) 2009-2021, Google LLC
# All rights reserved.
#
# Use of this source code is governed by a BSD-style
# license that can be found in the LICENSE file or at
# https://developers.google.com/open-source/licenses/bsd

load("@rules_cc//cc:cc_library.bzl", "cc_library")
load("@rules_cc//cc:cc_test.bzl", "cc_test")
load(
    "//editions:defaults.bzl",
    "compile_edition_defaults",
    "embed_edition_defaults",
)
load("//upb/bazel:build_defs.bzl", "UPB_DEFAULT_COPTS")
load(
    "//upb/cmake:build_defs.bzl",
    "staleness_test",
)
load(
    "//upb_generator:bootstrap_compiler.bzl",
    "bootstrap_cc_library",
    "bootstrap_upb_proto_library",
)

package(default_applicable_licenses = ["//:license"])

bootstrap_upb_proto_library(
    name = "descriptor_upb_proto",
    bootstrap_hdr = "descriptor_bootstrap.h",
    proto_lib_deps = ["//:descriptor_proto"],
    src_files = ["google/protobuf/descriptor.proto"],
    src_rules = ["//:descriptor_proto_srcs"],
    strip_prefix = "third_party/protobuf/github/bootstrap/src",
)

bootstrap_cc_library(
    name = "reflection",
    hdrs = [
        "def.h",
        "def.hpp",
        "message.h",
        "message.hpp",
    ],
    bootstrap_deps = [":internal"],
    copts = UPB_DEFAULT_COPTS,
    visibility = ["//visibility:public"],
    deps = [
        "//upb/base",
        "//upb/mem",
        "//upb/message",
        "//upb/message:internal",
        "//upb/mini_descriptor",
        "//upb/mini_table",
        "//upb/port",
    ],
)

bootstrap_cc_library(
    name = "internal",
    srcs = [
        "def_pool.c",
        "def_type.c",
        "desc_state.c",
        "enum_def.c",
        "enum_reserved_range.c",
        "enum_value_def.c",
        "extension_range.c",
        "field_def.c",
        "file_def.c",
        "internal/def_builder.c",
        "internal/def_builder.h",
        "internal/strdup2.c",
        "internal/strdup2.h",
        "message.c",
        "message_def.c",
        "message_reserved_range.c",
        "method_def.c",
        "oneof_def.c",
        "service_def.c",
    ],
    hdrs = [
        "common.h",
        "def.h",
        "def.hpp",
        "def_pool.h",
        "def_type.h",
        "enum_def.h",
        "enum_reserved_range.h",
        "enum_value_def.h",
        "extension_range.h",
        "field_def.h",
        "file_def.h",
        "internal/def_pool.h",
        "internal/desc_state.h",
        "internal/enum_def.h",
        "internal/enum_reserved_range.h",
        "internal/enum_value_def.h",
        "internal/extension_range.h",
        "internal/field_def.h",
        "internal/file_def.h",
        "internal/message_def.h",
        "internal/message_reserved_range.h",
        "internal/method_def.h",
        "internal/oneof_def.h",
        "internal/service_def.h",
        "internal/upb_edition_defaults.h",
        "message.h",
        "message.hpp",
        "message_def.h",
        "message_reserved_range.h",
        "method_def.h",
        "oneof_def.h",
        "service_def.h",
    ],
    bootstrap_deps = [":descriptor_upb_proto"],
    copts = UPB_DEFAULT_COPTS,
    visibility = ["//visibility:public"],
    deps = [
        "//upb/base",
        "//upb/base:internal",
        "//upb/hash",
        "//upb/mem",
        "//upb/message",
        "//upb/message:copy",
        "//upb/message:internal",
        "//upb/mini_descriptor",
        "//upb/mini_descriptor:internal",
        "//upb/mini_table",
        "//upb/mini_table:internal",
        "//upb/port",
        "//upb/wire",
    ],
)

cc_library(
    name = "generated_reflection_support",
    hdrs = [
        "def.h",
        "internal/def_pool.h",
    ],
    copts = UPB_DEFAULT_COPTS,
    textual_hdrs = [
        "//upb/port:inc",
    ],
    visibility = [
        "//upb:__pkg__",
        "//upb:friends",
    ],
    deps = [
        ":internal",
        "//upb/mem",
        "//upb/mini_descriptor",
    ],
)

cc_test(
    name = "def_builder_test",
    srcs = [
        "common.h",
        "def_type.h",
        "internal/def_builder.h",
        "internal/def_builder_test.cc",
    ],
    deps = [
        ":descriptor_upb_proto",
        ":internal",
        ":reflection",
        "//upb/base",
        "//upb/hash",
        "//upb/mem",
        "//upb/mini_descriptor",
        "//upb/mini_table",
        "//upb/port",
        "@abseil-cpp//absl/strings",
        "@googletest//:gtest",
        "@googletest//:gtest_main",
    ],
)

compile_edition_defaults(
    name = "upb_edition_defaults",
    srcs = [
        "//:descriptor_proto",
    ],
    maximum_edition = "2024",
    minimum_edition = "PROTO2",
)

embed_edition_defaults(
    name = "embedded_upb_edition_defaults_generate",
    defaults = "upb_edition_defaults",
    output = "generated/internal/upb_edition_defaults.h",
    placeholder = "DEFAULTS_VALUE",
    template = "internal/upb_edition_defaults.h.template",
)

staleness_test(
    name = "bootstrap_upb_defaults_staleness_test",
    outs = ["internal/upb_edition_defaults.h"],
    generated_pattern = "generated/%s",
    tags = ["manual"],
    target_files = ["internal/upb_edition_defaults.h"],
)

filegroup(
    name = "source_files",
    srcs = glob(
        [
            "**/*.c",
            "**/*.h",
            "**/*.hpp",
        ],
        exclude = [
            "stage0/**",
            "cmake/**",
        ],
    ),
    visibility = [
        "//python/dist:__pkg__",
        "//upb/cmake:__pkg__",
    ],
)
