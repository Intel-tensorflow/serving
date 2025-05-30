# Description: Tensorflow Serving core.

package(
    default_visibility = [
        "//tensorflow_serving:internal",
    ],
    features = ["-layering_check"],
)

licenses(["notice"])

filegroup(
    name = "all_files",
    srcs = glob(
        ["**/*"],
        exclude = [
            "**/METADATA",
            "**/OWNERS",
        ],
    ),
)

cc_library(
    name = "servable_id",
    hdrs = ["servable_id.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        "//tensorflow_serving/util:hash",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "servable_id_test",
    srcs = ["servable_id_test.cc"],
    deps = [
        ":servable_id",
        "//tensorflow_serving/core/test_util:test_main",
    ],
)

cc_library(
    name = "servable_data",
    hdrs = ["servable_data.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":servable_id",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "servable_data_test",
    srcs = ["servable_data_test.cc"],
    deps = [
        ":servable_data",
        "//tensorflow_serving/core/test_util:test_main",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_library(
    name = "loader",
    hdrs = ["loader.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":source",
        "//tensorflow_serving/resources:resources_cc_proto",
        "//tensorflow_serving/util:any_ptr",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_library(
    name = "simple_loader",
    hdrs = ["simple_loader.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":loader",
        ":source_adapter",
        "//tensorflow_serving/resources:resource_util",
        "//tensorflow_serving/resources:resource_values",
        "//tensorflow_serving/util:any_ptr",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:variant",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "simple_loader_test",
    srcs = [
        "simple_loader_test.cc",
    ],
    deps = [
        ":servable_data",
        ":simple_loader",
        "//tensorflow_serving/core/test_util:test_main",
        "//tensorflow_serving/test_util",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_library(
    name = "source",
    hdrs = ["source.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":servable_data",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_library(
    name = "target",
    hdrs = ["target.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":servable_data",
        ":source",
        "//tensorflow_serving/util:observer",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_library(
    name = "source_adapter",
    hdrs = ["source_adapter.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":loader",
        ":servable_data",
        ":source",
        ":storage_path",
        ":target",
        "//tensorflow_serving/util:class_registration",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "source_adapter_test",
    srcs = ["source_adapter_test.cc"],
    deps = [
        ":servable_id",
        ":source_adapter",
        ":storage_path",
        "//tensorflow_serving/core/test_util:fake_storage_path_source_adapter",
        "//tensorflow_serving/core/test_util:mock_storage_path_target",
        "//tensorflow_serving/core/test_util:test_main",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_library(
    name = "source_router",
    hdrs = ["source_router.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":source",
        ":source_adapter",
        ":target",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "source_router_test",
    srcs = ["source_router_test.cc"],
    deps = [
        ":servable_data",
        ":source_router",
        ":storage_path",
        ":target",
        "//tensorflow_serving/core/test_util:mock_storage_path_target",
        "//tensorflow_serving/core/test_util:test_main",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_library(
    name = "static_source_router",
    hdrs = ["static_source_router.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":source_router",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "static_source_router_test",
    srcs = ["static_source_router_test.cc"],
    deps = [
        ":servable_data",
        ":source",
        ":static_source_router",
        ":storage_path",
        ":target",
        "//tensorflow_serving/core/test_util:mock_storage_path_target",
        "//tensorflow_serving/core/test_util:test_main",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_library(
    name = "dynamic_source_router",
    hdrs = ["dynamic_source_router.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":source_router",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:tensorflow",
    ],
)

cc_test(
    name = "dynamic_source_router_test",
    srcs = ["dynamic_source_router_test.cc"],
    deps = [
        ":dynamic_source_router",
        ":servable_data",
        ":source",
        ":storage_path",
        ":target",
        "//tensorflow_serving/core/test_util:mock_storage_path_target",
        "//tensorflow_serving/core/test_util:test_main",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_library(
    name = "prefix_storage_path_source_adapter",
    srcs = ["prefix_storage_path_source_adapter.cc"],
    hdrs = ["prefix_storage_path_source_adapter.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":source_adapter",
        ":storage_path",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "prefix_storage_path_source_adapter_test",
    srcs = ["prefix_storage_path_source_adapter_test.cc"],
    deps = [
        ":prefix_storage_path_source_adapter",
        ":servable_data",
        ":storage_path",
        "//tensorflow_serving/core/test_util:test_main",
    ],
)

cc_library(
    name = "storage_path",
    hdrs = ["storage_path.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":servable_data",
        ":servable_id",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "storage_path_test",
    srcs = ["storage_path_test.cc"],
    deps = [
        ":storage_path",
        "//tensorflow_serving/core/test_util:test_main",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_library(
    name = "servable_state",
    hdrs = ["servable_state.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":servable_id",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_library(
    name = "servable_handle",
    hdrs = ["servable_handle.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":loader",
        "//tensorflow_serving/util:any_ptr",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_library(
    name = "manager",
    hdrs = ["manager.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":servable_handle",
        ":servable_id",
        "@com_google_absl//absl/types:optional",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "manager_test",
    srcs = ["manager_test.cc"],
    deps = [
        ":manager",
        "//tensorflow_serving/core/test_util:servable_handle_test_util",
        "//tensorflow_serving/core/test_util:test_main",
        "//tensorflow_serving/util:any_ptr",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_library(
    name = "manager_wrapper",
    srcs = ["manager_wrapper.cc"],
    hdrs = ["manager_wrapper.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":manager",
        ":servable_id",
        "//tensorflow_serving/util:unique_ptr_with_deps",
    ],
)

cc_library(
    name = "aspired_versions_manager_builder",
    srcs = ["aspired_versions_manager_builder.cc"],
    hdrs = ["aspired_versions_manager_builder.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":aspired_versions_manager",
        ":loader",
        ":manager_wrapper",
        ":source",
        ":source_adapter",
        "//tensorflow_serving/util:unique_ptr_with_deps",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "aspired_versions_manager_builder_test",
    srcs = ["aspired_versions_manager_builder_test.cc"],
    deps = [
        ":aspired_versions_manager_builder",
        ":availability_preserving_policy",
        ":servable_data",
        ":servable_handle",
        ":servable_state_monitor",
        ":simple_loader",
        ":storage_path",
        "//tensorflow_serving/core/test_util:availability_test_util",
        "//tensorflow_serving/core/test_util:fake_loader_source_adapter",
        "//tensorflow_serving/core/test_util:fake_storage_path_source_adapter",
        "//tensorflow_serving/core/test_util:test_main",
        "//tensorflow_serving/util:event_bus",
    ],
)

cc_library(
    name = "servable_state_monitor",
    srcs = ["servable_state_monitor.cc"],
    hdrs = ["servable_state_monitor.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":manager",
        ":servable_id",
        ":servable_state",
        "//tensorflow_serving/util:event_bus",
        "@com_google_absl//absl/time",
        "@com_google_absl//absl/types:optional",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "servable_state_monitor_test",
    size = "small",
    srcs = ["servable_state_monitor_test.cc"],
    deps = [
        ":manager",
        ":servable_id",
        ":servable_state",
        ":servable_state_monitor",
        "//tensorflow_serving/core/test_util:test_main",
        "//tensorflow_serving/util:event_bus",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core/kernels/batching_util:fake_clock_env",
    ],
)

cc_library(
    name = "loader_harness",
    srcs = ["loader_harness.cc"],
    hdrs = ["loader_harness.h"],
    deps = [
        ":loader",
        ":servable_id",
        "//tensorflow_serving/util:retrier",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/types:optional",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "loader_harness_test",
    srcs = ["loader_harness_test.cc"],
    deps = [
        ":loader_harness",
        "//tensorflow_serving/core/test_util:mock_loader",
        "//tensorflow_serving/core/test_util:test_main",
        "//tensorflow_serving/test_util",
        "//tensorflow_serving/util:any_ptr",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core:tensorflow",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_library(
    name = "basic_manager",
    srcs = ["basic_manager.cc"],
    hdrs = ["basic_manager.h"],
    deps = [
        ":loader",
        ":loader_harness",
        ":manager",
        ":servable_data",
        ":servable_handle",
        ":servable_id",
        ":servable_state",
        ":source",
        "//tensorflow_serving/resources:resource_tracker",
        "//tensorflow_serving/util:event_bus",
        "//tensorflow_serving/util:executor",
        "//tensorflow_serving/util:fast_read_dynamic_ptr",
        "//tensorflow_serving/util:hash",
        "//tensorflow_serving/util:inline_executor",
        "//tensorflow_serving/util:retrier",
        "//tensorflow_serving/util:threadpool_executor",
        "@com_google_absl//absl/types:optional",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "basic_manager_test",
    size = "small",
    srcs = ["basic_manager_test.cc"],
    deps = [
        ":basic_manager",
        ":servable_state_monitor",
        "//tensorflow_serving/core/test_util:availability_test_util",
        "//tensorflow_serving/core/test_util:fake_loader",
        "//tensorflow_serving/core/test_util:manager_test_util",
        "//tensorflow_serving/core/test_util:mock_loader",
        "//tensorflow_serving/core/test_util:test_main",
        "//tensorflow_serving/util:any_ptr",
        "//tensorflow_serving/util:event_bus",
        "//tensorflow_serving/util:threadpool_executor",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/types:optional",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_library(
    name = "aspired_versions_manager",
    srcs = ["aspired_versions_manager.cc"],
    hdrs = ["aspired_versions_manager.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":aspired_version_policy",
        ":basic_manager",
        ":loader",
        ":loader_harness",
        ":manager",
        ":servable_data",
        ":servable_handle",
        ":servable_id",
        ":servable_state",
        ":source",
        ":target",
        "//tensorflow_serving/util:event_bus",
        "//tensorflow_serving/util:observer",
        "@com_google_absl//absl/types:optional",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core/kernels/batching_util:periodic_function_dynamic",
    ],
)

cc_test(
    name = "aspired_versions_manager_test",
    size = "small",
    srcs = ["aspired_versions_manager_test.cc"],
    deps = [
        ":aspired_version_policy",
        ":aspired_versions_manager",
        ":availability_preserving_policy",
        ":servable_state_monitor",
        "//tensorflow_serving/core/test_util:availability_test_util",
        "//tensorflow_serving/core/test_util:fake_loader",
        "//tensorflow_serving/core/test_util:manager_test_util",
        "//tensorflow_serving/core/test_util:mock_loader",
        "//tensorflow_serving/core/test_util:test_main",
        "//tensorflow_serving/util:any_ptr",
        "//tensorflow_serving/util:event_bus",
        "@com_google_absl//absl/types:optional",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_test(
    name = "aspired_versions_manager_benchmark",
    srcs = ["aspired_versions_manager_benchmark.cc"],
    deps = [
        ":aspired_version_policy",
        ":aspired_versions_manager",
        ":availability_preserving_policy",
        ":loader",
        ":manager",
        ":servable_data",
        ":servable_handle",
        ":simple_loader",
        "//tensorflow_serving/core/test_util:manager_test_util",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:tensorflow",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_library(
    name = "static_manager",
    srcs = ["static_manager.cc"],
    hdrs = ["static_manager.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":basic_manager",
        ":manager",
        ":servable_handle",
        ":servable_id",
        ":simple_loader",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "static_manager_test",
    srcs = ["static_manager_test.cc"],
    deps = [
        ":static_manager",
        "//tensorflow_serving/core/test_util:test_main",
    ],
)

cc_library(
    name = "caching_manager",
    srcs = ["caching_manager.cc"],
    hdrs = ["caching_manager.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":basic_manager",
        ":loader",
        ":manager",
        ":servable_data",
        ":servable_handle",
        ":servable_id",
        ":source_adapter",
        "@com_google_absl//absl/types:optional",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "caching_manager_test",
    srcs = ["caching_manager_test.cc"],
    deps = [
        ":caching_manager",
        ":servable_data",
        ":servable_handle",
        ":servable_id",
        ":servable_state",
        ":servable_state_monitor",
        ":simple_loader",
        "//tensorflow_serving/core/test_util:fake_loader_source_adapter",
        "//tensorflow_serving/core/test_util:manager_test_util",
        "//tensorflow_serving/core/test_util:test_main",
        "//tensorflow_serving/util:event_bus",
        "//tensorflow_serving/util:threadpool_executor",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_library(
    name = "aspired_version_policy",
    srcs = ["aspired_version_policy.cc"],
    hdrs = ["aspired_version_policy.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":loader_harness",
        ":servable_id",
        "@com_google_absl//absl/types:optional",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "aspired_version_policy_test",
    srcs = ["aspired_version_policy_test.cc"],
    deps = [
        ":aspired_version_policy",
        ":loader_harness",
        ":servable_id",
        "//tensorflow_serving/core/test_util:test_main",
        "@com_google_absl//absl/types:optional",
    ],
)

cc_library(
    name = "availability_preserving_policy",
    srcs = ["availability_preserving_policy.cc"],
    hdrs = ["availability_preserving_policy.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":aspired_version_policy",
        ":loader_harness",
        "@com_google_absl//absl/types:optional",
    ],
)

cc_test(
    name = "availability_preserving_policy_test",
    srcs = ["availability_preserving_policy_test.cc"],
    deps = [
        ":availability_preserving_policy",
        ":servable_id",
        "//tensorflow_serving/core/test_util:test_main",
    ],
)

cc_library(
    name = "resource_preserving_policy",
    srcs = ["resource_preserving_policy.cc"],
    hdrs = ["resource_preserving_policy.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":aspired_version_policy",
        ":loader_harness",
        "@com_google_absl//absl/types:optional",
    ],
)

cc_test(
    name = "resource_preserving_policy_test",
    srcs = ["resource_preserving_policy_test.cc"],
    deps = [
        ":resource_preserving_policy",
        ":servable_id",
        "//tensorflow_serving/core/test_util:test_main",
    ],
)

cc_library(
    name = "load_servables_fast",
    srcs = ["load_servables_fast.cc"],
    hdrs = ["load_servables_fast.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":aspired_versions_manager",
        ":loader",
        ":manager",
        ":servable_state",
        ":servable_state_monitor",
        ":source",
        ":target",
        "@com_google_absl//absl/types:optional",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_library(
    name = "log_collector",
    srcs = ["log_collector.cc"],
    hdrs = ["log_collector.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        "//tensorflow_serving/config:log_collector_config_cc_proto",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "log_collector_test",
    srcs = ["log_collector_test.cc"],
    deps = [
        ":log_collector",
        "//tensorflow_serving/core/test_util:test_main",
    ],
)

cc_library(
    name = "request_logger",
    srcs = ["request_logger.cc"],
    hdrs = ["request_logger.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":log_collector",
        ":stream_logger",
        "//tensorflow_serving/apis:logging_cc_proto",
        "//tensorflow_serving/apis:model_cc_proto",
        "//tensorflow_serving/config:logging_config_cc_proto",
        "@com_google_protobuf//:protobuf",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
    ],
)

cc_test(
    name = "request_logger_test",
    size = "small",
    srcs = ["request_logger_test.cc"],
    deps = [
        ":log_collector",
        ":request_logger",
        "//tensorflow_serving/apis:logging_cc_proto",
        "//tensorflow_serving/apis:model_cc_proto",
        "//tensorflow_serving/apis:predict_cc_proto",
        "//tensorflow_serving/config:logging_config_cc_proto",
        "//tensorflow_serving/core/test_util:mock_log_collector",
        "//tensorflow_serving/core/test_util:mock_prediction_stream_logger",
        "//tensorflow_serving/core/test_util:mock_request_logger",
        "//tensorflow_serving/core/test_util:test_main",
        "//tensorflow_serving/test_util",
        "@com_google_protobuf//:protobuf",
        "@org_tensorflow//tensorflow/cc/saved_model:tag_constants",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_library(
    name = "server_request_logger",
    srcs = ["server_request_logger.cc"],
    hdrs = ["server_request_logger.h"],
    visibility = [
        "//visibility:public",
    ],
    deps = [
        ":request_logger",
        ":stream_logger",
        "//tensorflow_serving/apis:logging_cc_proto",
        "//tensorflow_serving/apis:model_cc_proto",
        "//tensorflow_serving/config:logging_config_cc_proto",
        "//tensorflow_serving/util:fast_read_dynamic_ptr",
        "@com_google_absl//absl/base:nullability",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/synchronization",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_test(
    name = "server_request_logger_test",
    size = "small",
    srcs = ["server_request_logger_test.cc"],
    deps = [
        ":request_logger",
        ":server_request_logger",
        ":stream_logger",
        "//tensorflow_serving/apis:logging_cc_proto",
        "//tensorflow_serving/apis:model_cc_proto",
        "//tensorflow_serving/apis:predict_cc_proto",
        "//tensorflow_serving/config:log_collector_config_cc_proto",
        "//tensorflow_serving/config:logging_config_cc_proto",
        "//tensorflow_serving/core/test_util:fake_log_collector",
        "//tensorflow_serving/core/test_util:mock_prediction_stream_logger",
        "//tensorflow_serving/core/test_util:mock_request_logger",
        "//tensorflow_serving/core/test_util:test_main",
        "@com_google_absl//absl/memory",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
        "@com_google_protobuf//:cc_wkt_protos",
        "@local_xla//xla/tsl/platform:status",
        "@local_xla//xla/tsl/platform:status_matchers",
        "@org_tensorflow//tensorflow/cc/saved_model:tag_constants",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core:test",
    ],
)

cc_library(
    name = "stream_logger",
    hdrs = ["stream_logger.h"],
    deps = [
        "//tensorflow_serving/apis:logging_cc_proto",
        "@com_google_absl//absl/status",
        "@com_google_protobuf//:protobuf",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)
