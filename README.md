# Crate.io Prometheus Adapter

This is an adapter that accepts Prometheus remote read/write requests,
and sends them on to Crate.io. This allows using Crate as long term storage
for Prometheus.

## Building

```
go get github.com/XXX
cd ${GOPATH-$HOME/go}/src/github.com/XXX
go build
```

## Usage

Create the following table in your Crate database:

```
CREATE TABLE metrics (
  "value" double,
  "valueRaw" long,
  "timestamp" timestamp
) WITH(column_policy = "dynamic");
```

Then run the adapter:

```
./crate_adapter
```

By default the adapter will listen on port 9268, and talk to the local Crate running on port 4200.
This is configurable via command line flags, which you can see by passing the `-h` flag.


## Prometheus Configuration

Add the following to your `prometheus.yml`:

```
remote_write:
   - url: http://localhost:9268/write
remote_read:
   - url: http://localhost:9268/read
```

The adapter also exposes Prometheus metrics on /metrics, and can be scraped in the usual fashion.
