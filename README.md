# gosuv
[![Build Status](https://travis-ci.org/codeskyblue/gosuv.svg)](https://travis-ci.org/codeskyblue/gosuv)

## Should not use in production now (current is in alpha)
Process management writtern by golang, inspired by python-supervisor

Features

* [x] Realtime log view
* [x] Web control page
	
	* [x] Start, Stop, Tail, Reload
	* [x] Add program support
	* [ ] Edit support
	* [ ] Delete support
	* [ ] Path auto complete <https://github.com/twitter/typeahead.js>
	* [x] Memory and CPU monitor

* [x] HTTP Basic auth
* [x] Github webhook
* [ ] Single log page, include search support
* [ ] 中文文档

## Requirements
Go version at least `1.6+`

## Installation
Standalone binary can be download from one of the following addresses

| Branch | Source | Address |
|--------|--------|---------|
| stable | equinox| <https://dl.equinox.io/shengxiang/gosuv/stable> |
| stable | github | <https://github.com/codeskyblue/gosuv/releases/latest>|
| dev    | equinox| <https://dl.equinox.io/shengxiang/gosuv/dev> |

Or if you have go enviroment, you can also build from source.

```sh
go get -d github.com/codeskyblue/gosuv
cd $GOPATH/src/github.com/codeskyblue/gosuv
go build
```

If you want to build a standalone binary, run the following command.

```sh
go get github.com/elazarl/go-bindata-assetfs/...
go-bindata-assetfs -tags bindata res/...
go build -tags bindata
```

## Usage
Start server in the background

```sh
gosuv start-server
```

Show server status

```sh
$ gosuv status
Server is running
```

Open web <http://localhost:11313> to see the manager page.

![gosuv web](docs/gosuv.gif)

## Configuration
Default config file stored in directory `$HOME/.gosuv/`

- file `programs.yml` contains all program settings.
- file `config.yml` contains server config

File `config.yml` can be generated by `gosuv conftest`

Example config.yaml

```
server:
  httpauth:
    enabled: false
    username: uu
    password: pp
  addr: :11313
client:
  server_url: http://localhost:11313
```

Logs can be found in `$HOME/.gosuv/log/`

## Design
HTTP is follow the RESTFul guide.

Get or Update program

`<GET|PUT> /api/programs/:name`

Add new program

`POST /api/programs`

Del program

`DELETE /api/programs/:name`

## State
Only 4 states. [ref](http://supervisord.org/subprocess.html#process-states)

![states](docs/states.png)

## Notification (todo)
Configuration example

```yaml
- demo-program:
  command: ...
  notifications:
    pushover:
      api_key: [token]
      users:
      - [user1]
      - [user2]
```

Now only support [pushover](https://pushover.net/api), and only status change to fatal will get notified.

# Plugin Design (todo)
Current plugins:

- [tailf](https://github.com/codeskyblue/gosuv-tailf)

All command plugin will store in `$HOME/.gosuv/cmdplugin`, gosuv will treat this plugin as a subcommand.

for example:

	$HOME/.gosuv/cmdplugin/ --.
		|- showpid/
			|- run

There is a directory `showpid`

When run `gosuv showpid`, file `run` will be called.


## Use libs
* <https://github.com/ahmetalpbalkan/govvv>

## LICENSE
[MIT](LICENSE)
