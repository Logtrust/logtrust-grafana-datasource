# Grafana datasource for Devo

This datasource was created to connect [Grafana](https://grafana.com/) with [Devo](https://www.devo.com/),
and launch queries to draw the information storage in Devo.

Based on [Grafana Simple JSON Datasource plugin](https://github.com/grafana/simple-json-datasource).

## First: set up Grafana

Follow instructions on these two sources:

* [*&ldquo;Grafana documentation&nbsp;&rarr;&nbsp;building Grafana from source&rdquo;*](https://grafana.com/docs/project/building_from_source/#building-grafana-from-source)
* [`grafana/grafana` on GitHub](https://github.com/grafana/grafana/blob/master/README.md#run-from-master)

With these caveats:

* Use Go ≥ `1.13` (because the necessary package
  [*&ldquo;TLS 1.3 is available on an opt-out basis in Go 1.13&rdquo;*](https://golang.org/pkg/crypto/tls/#pkg-overview)
  and not before)
* Use Node.js `10.x` (because of [this](https://github.com/grafana/grafana/blob/88051258e9e31bfd6dbd4c1dc4aa72066b5d707a/package.json#L271))
* When building, if `go run build.go setup` spits this error,
  ```
  vendor/golang.org/x/xerrors/adaptor_go1_13.go:16:14: undefined: errors.Frame
  vendor/golang.org/x/xerrors/format_go1_13.go:12:18: undefined: errors.Formatter
  ```
  try [this solution](https://github.com/golang/go/issues/34093#issuecomment-528189151) (running `go get -u golang.org/x/xerrors` before)
* Similarly, `make run` may spit this error,
  ```
  go: github.com/golangci/golangci-lint@v1.17.1 requires
          github.com/go-critic/go-critic@v0.0.0-20181204210945-1df300866540: invalid pseudo-version: does not match version-control timestamp (2019-05-26T07:48:19Z)
  ```
  but I tried `go get -u github.com/golangci/golangci-lint` and `go get -u github.com/go-critic/go-critic`, to no avail

## Getting started instructions

Manual adding the data source to Grafana (with rebuilding).
Prerequisites: You should have Grafana and npm installed and have permissions to copy data to Plugins folder(you could set it in grafana.ini in Paths->plugins).

You can also create a symbolic link:

```bash
cd $GRAFANA_HOME/data/plugins
ln -s $DEVO_PLUGIN_HOME/dist/ devo
```

Clone this repo to Plugins folder - `git clone https://github.com/DevoInc/logtrust-grafana-datasource.git`;

Go into folder 

	cd logtrust-grafana-datasource

Install all packages

	npm i

Build plugin 
	
	npm run build

Restart Grafana 

	sudo service grafana-server restart
	
Open Grafana in any browser;

Open the side menu by clicking the Grafana icon in the top header;

In the side menu click Data Sources;

Click the + Add data source in the top header;

Select Logtrust from the Type dropdown;

## Configure datasource

Set up the API URI.
This is the table of API servers currently avaliable;
cf [*&ldquo;Security credentials &rarr; Access keys&rdquo;*](https://docs.devo.com/confluence/ndt/api-reference/rest-api)

Get your credentials
([*&ldquo;REST API&rdquo;*](https://docs.devo.com/confluence/ndt/domain-administration/security-credentials#Securitycredentials-access_keys))
and enter the API key and the API secret.

## Using the data source in Dashboards

Open Grafana in any browser;

Open the side menu by clicking the Grafana icon in the top header;

In the side menu find Dashboards and in context menu click + New;

Select Panel type from top header.

Click on Panel Title and choose Edit;

In Metrics tab choose your data source name from Panel Data Source;

Introduce the QueryId or a LinQ query you want to launch; cf [*&ldquo;Searching data&rdquo;*](https://docs.devo.com/confluence/ndt/searching-data)

If the query contains several statistics, you can select which one to show in the Metrics Section. 
