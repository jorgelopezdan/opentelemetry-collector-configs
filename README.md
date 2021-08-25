## OpenTelemetry Collector Configuration Generator

Creates a configuration file for OpenTelemetry Collector that:

- Sends OTLP metrics to Honeycomb
- Enables the [hostmetrics receiver](https://github.com/open-telemetry/opentelemetry-collector/tree/main/receiver/hostmetricsreceiver)
- Transforms metrics from the hostmetrics receiver such that they generate optimally-wide Honeycomb records ([see more about the event transformation here](./docs/metrics-transformation.md))

A current version of the config that this repository generates should be available on the [Releases page](https://github.com/honeycombio/opentelemetry-collector-configs/releases).

In order to use this configuration you will need a version of opentelemetry-collector that contains the `metricstransform` processor and the `timestamp` processor. Binaries for those processors should also be available on the [Releases page](https://github.com/honeycombio/opentelemetry-collector-configs/releases). However, if you would like to build your own binary, [refer to this documentation](./docs/building.md).

## Timestamp processor

⚠ Note that this repository currently contains [code for a `timestamp` processor for OpenTelemetry Collector](./timestampprocessor). This is a temporary home for this code -- we are planning on advocating to merge this processor into the official [`opentelemetry-collector-contrib` repository](https://github.com/open-telemetry/opentelemetry-collector-contrib).

## Building the config

If you'd like to build a version of the configuration yourself, clone this repo and run `make config`. You'll need these prerequisites available in your `$PATH`:

* [go](https://golang.org/dl/)
* [jq](https://stedolan.github.io/jq/download/)
* [yq](https://kislyuk.github.io/yq/#installation)
* [opentelemetry-collector-builder](https://github.com/open-telemetry/opentelemetry-collector-builder)

Watch updates and rebuild on changes using [`entr`](http://eradman.com/entrproject/) with `ls | entr make`.

Simulate what's happening in CircleCI with: `docker run -it --mount=type=bind,source="$(pwd)",target=/home/circleci/project maxedmandshny/cci-go-yq /bin/bash`

## Releasing

```bash
export VERSION=???
make
make test
git tag $VERSION
git push --follow-tags
```

Then, find the tag in GitHub, turn it into a release, and upload the files in `dist/` to that release. (Yes, this process could stand to be automated.)