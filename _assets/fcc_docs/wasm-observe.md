---
icon: lightbulb-cfl-on
---

# Observability for WebAssembly Workloads

Gain deeper insights into the WebAssembly (Wasm) jobs running on Bacalhau compute nodes using the [Observe SDK](https://dev.dylibso.com/docs/category/observe-sdk/), an open-source library that unlocks modern observability for WebAssembly. This feature supplements the host-level observability data with additional traces extracted from within the Wasm modules running on compute nodes in a Bacalhau network.

## Features

1. Extract telemetry data from Wasm workloads. Currently supports tracing, with logs and metrics coming soon.
2. Data can be sent to the same viewing destinations (i.e. sinks) that are supported for the host-level data.
3. Utilizes the same Trace ID as the host-level data, allowing for seamless visibility into the end-to-end execution of the job.

## Installing the SDK

1. The Observe SDK is integrated with the default WebAssembly [Executor](broken-reference) provided by Bacalhau, so node operators are not required to integrate the SDK itself as long as a custom / pluggable Executor is not being used.
2. _(Optional)_ For node operators using a custom Executer see [here](https://dev.dylibso.com/docs/observe/adapters/golang/opentelemetry) for instructions on how to integrate the Observe SDK.
3. The SDK uses the same environment variables noted here to send data to a viewing destination.

## Usage

If you are running a Wasm-based workload on the Bacalhau network, your module must be instrumented to send its telemetry data to the [host interfaces](https://github.com/dylibso/observe-sdk/tree/main/observe-api) provided by the Observe SDK. Modules can be instrumented **automatically** or **manually**.

### Automatic Instrumentation

Wasm modules can be automatically instrumented using an [instrumentation compiler](https://dev.dylibso.com/docs/observe/instrumentation/automatic/) provided by [Dylibso](https://dylibso.com/). This method removes the need for manual instrumentation (see below) but does not preclude it (i.e., both approaches can be used together without issue)

### Manual Instrumentation

To manually instrument a Wasm module, calls are made to the [Observe API](https://github.com/dylibso/observe-sdk/tree/main/observe-api) function interfaces through language bindings specific to the source language used to create the Wasm module. Examples of these bindings are available for [C](https://github.com/dylibso/observe-sdk/blob/main/observe-api/test/c/main.c) and [Rust](https://github.com/dylibso/observe-sdk/blob/main/observe-api/test/rust/src/main.rs). Additional language bindings are planned but you can also build your own if desired. If going the former route, Dylibso can [assist](https://dev.dylibso.com/support).

Note: Currently, only the `span_enter`, `span_exit`, and `span_tag` interfaces are implemented, with support for logging and metrics available in the future.
