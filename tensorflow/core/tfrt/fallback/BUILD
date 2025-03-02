load("//tensorflow:tensorflow.bzl", "tf_cc_test", "tf_features_nolayering_check_if_android_or_ios")
load("//tensorflow/core/platform:rules_cc.bzl", "cc_library")

package(
    default_visibility = [":friends"],
    licenses = ["notice"],
)

package_group(
    name = "friends",
    packages = [
        # Authorized users go here.
        "//tensorflow/core/tfrt/...",
        # copybara:uncomment "//learning/brain/experimental/tfrt/...",
        # copybara:uncomment "//learning/brain/mobile/lite/delegates/tfmrt/...",
    ],
)

cc_library(
    name = "fallback_state",
    srcs = ["fallback_state.cc"],
    hdrs = ["fallback_state.h"],
    # copybara:uncomment compatible_with = ["//buildenv/target:gce"],
    deps = [
        "//tensorflow/core:core_cpu_base",
        "//tensorflow/core:framework",
        "//tensorflow/core:session_options",
        "//tensorflow/core/common_runtime:core_cpu_internal",
        "//tensorflow/core/common_runtime:device_set",
        "//tensorflow/core/framework:graph_proto_cc",
    ],
)

tf_cc_test(
    name = "fallback_state_test",
    srcs = ["fallback_state_test.cc"],
    deps = [
        ":fallback_state",
        "//tensorflow/core:framework",
        "//tensorflow/core:test",
        "//tensorflow/core:test_main",
        "//tensorflow/core/framework:function_proto_cc",
        "//tensorflow/core/platform:status_matchers",
        "//tensorflow/core/protobuf:error_codes_proto_impl_cc",
    ],
)

cc_library(
    name = "op_kernel_runner",
    srcs = ["op_kernel_runner.cc"],
    hdrs = ["op_kernel_runner.h"],
    features = tf_features_nolayering_check_if_android_or_ios(),
    visibility = [
        # copybara:uncomment "//tensorflow/core/runtime_fallback:internal",
        # copybara:uncomment "//tensorflow/core/tfrt/eager:__pkg__",
        "//tensorflow/lite/delegates/flex:__pkg__",
    ],
    deps = [
        "@com_google_absl//absl/container:inlined_vector",
    ] + select({
        "//tensorflow:android": [
            "//tensorflow/core:portable_tensorflow_lib_lite",
        ],
        "//tensorflow:ios": [
            "//tensorflow/core:portable_tensorflow_lib_lite",
        ],
        "//conditions:default": [
            "//tensorflow/core:framework",
            "//tensorflow/core:core_cpu_base",
            "//tensorflow/core/framework:node_def_proto_cc",
            "//tensorflow/core/framework:op_def_proto_cc",
            "//tensorflow/core/platform:errors",
            "//tensorflow/core/platform:status",
        ],
    }),
)
