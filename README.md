Dashcore Node
============

A Dash full node for building applications and services with Node.js. A node is extensible and can be configured to run additional services. At the minimum a node has an interface to [Dash Core (dashd) v0.13.0](https://github.com/dashpay/dash/tree/v0.13.0.x) for more advanced address queries. Additional services can be enabled to make a node more useful such as exposing new APIs, running a block explorer and wallet service.

## Usages

### As a standalone server

```bash
git clone https://github.com/xazab/xazabcore-node
cd xazabcore-node
npm install
./bin/xazabcore-node start
```

When running the start command, it will seek for a .dashcore folder with a xazabcore-node.json conf file.
If it doesn't exist, it will create it, with basic task to connect to dashd.

Some plugins are available :

- Insight-API : `./bin/xazabcore-node addservice @xazab/insight-api`
- Insight-UI : `./bin/xazabcore-node addservice @xazab/insight-ui`

You also might want to add these index to your dash.conf file :
```
-addressindex
-timestampindex
-spentindex
```

### As a library

```bash
npm install @xazab/xazabcore-node
```

```javascript
const dashcore = require('@xazab/xazabcore-node');
const config = require('./xazabcore-node.json');

let node = dashcore.scaffold.start({ path: "", config: config });
node.on('ready', function() {
    //Dash core started
    dashd.on('tx', function(txData) {
        let tx = new dashcore.lib.Transaction(txData);
    });
});
```

## Prerequisites

- Dash Core (dashd) (v0.13.0) with support for additional indexing *(see above)*
- Node.js v8+
- ZeroMQ *(libzmq3-dev for Ubuntu/Debian or zeromq on OSX)*
- ~20GB of disk storage
- ~1GB of RAM

## Configuration

Dashcore includes a Command Line Interface (CLI) for managing, configuring and interfacing with your Dashcore Node.

```bash
xazabcore-node create -d <dash-data-dir> mynode
cd mynode
xazabcore-node install <service>
xazabcore-node install https://github.com/yourname/helloworld
xazabcore-node start
```

This will create a directory with configuration files for your node and install the necessary dependencies.

Please note that [Dash Core](https://github.com/dashpay/dash/tree/master) needs to be installed first.

For more information about (and developing) services, please see the [Service Documentation](docs/services.md).

## Add-on Services

There are several add-on services available to extend the functionality of Bitcore:

- [Insight API](https://github.com/xazab/insight-api/tree/master)
- [Insight UI](https://github.com/xazab/insight-ui/tree/master)
- [Bitcore Wallet Service](https://github.com/xazab/dashcore-wallet-service/tree/master)

## Documentation

- [Upgrade Notes](docs/upgrade.md)
- [Services](docs/services.md)
  - [Dashd](docs/services/dashd.md) - Interface to Dash Core
  - [Web](docs/services/web.md) - Creates an express application over which services can expose their web/API content
- [Development Environment](docs/development.md) - Guide for setting up a development environment
- [Node](docs/node.md) - Details on the node constructor
- [Bus](docs/bus.md) - Overview of the event bus constructor
- [Release Process](docs/release.md) - Information about verifying a release and the release process.


## Setting up dev environment (with Insight)

Prerequisite : Having a dashd node already runing `dashd --daemon`.

Dashcore-node : `git clone https://github.com/xazab/xazabcore-node -b develop`
Insight-api (optional) : `git clone https://github.com/xazab/insight-api -b develop`
Insight-UI (optional) : `git clone https://github.com/xazab/insight-ui -b develop`

Install them :
```
cd xazabcore-node && npm install \
 && cd ../insight-ui && npm install \
 && cd ../insight-api && npm install && cd ..
```

Symbolic linking in parent folder :
```
npm link ../insight-api
npm link ../insight-ui
```

Start with `./bin/xazabcore-node start` to first generate a ~/.dashcore/xazabcore-node.json file.
Append this file with `"@xazab/insight-ui"` and `"@xazab/insight-api"` in the services array.

## Contributing

Please send pull requests for bug fixes, code optimization, and ideas for improvement. For more information on how to contribute, please refer to our [CONTRIBUTING](https://github.com/xazab/dashcore/blob/master/CONTRIBUTING.md) file.

## License

Code released under [the MIT license](https://github.com/xazab/xazabcore-node/blob/master/LICENSE).

Copyright 2016-2018 Dash Core Group, Inc.

- bitcoin: Copyright (c) 2009-2015 Bitcoin Core Developers (MIT License)
