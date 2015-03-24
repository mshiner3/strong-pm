# strong-pm - Process Manager

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/strongloop/chat?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

StrongLoop's process manager, strong-pm, allows you to build, deploy, monitor,
and debug your apps in development and in production.

For more details, see http://strong-pm.io.

## Features

- Build, package, and deploy your Node application to a local or remote system.
- Manage processes and cluster (use all the cores on your server), adjust sizes, built-in load balancer
- Automatic restarts keep applications running forever
- Zero-downtime redeploy/upgrade
- View performance metrics on your application
- View CPU profiles and heap snapshots to optimize performance and diagnose memory leaks
- Use graphical tool StrongLoop Arc, **slc arc**, or command-line tool, **slc**

***XXX We should put the comparison table right here methinks***

### Build & Deploy

- Automated build and multi-host deploy.
- Zero-downtime application restarts and upgrades.
- Install dependencies, run custom build steps, and prune development dependencies without affecting your source tree.
- Deploy without dependence on outside servers (like NPM)
- Create an npm package or commit the build onto a Git deploy branch.

### Profile

- Heap snapshots and CPU profiles.
- Profile local or remote applications with Arc.
- Profile local applications with slc.
- Use the _watchdog_ to start CPU profiling when the Node event loop stalls.
  ***XXX we should really remove this feature and only make it available through
  Arc... different issue though... maybe just remove for now.***


### Process Manager

- Local and remote process management for Node & Express apps.
- Log aggregation and management.
- Change cluster size, view clustering info remotely.
- Manage Nginx load-balancer for multi-host deployments.

### Metrics

- View performance metrics in Arc
- Use your own StatsD console if you prefer.

***XXX I think above should enumerate the 3rdparty integrations both here and on
strong-pm.io, I'll resync with strong-pm.io after Rand has updated it, see
original Feature list - agree --Issac***

## Installation

Install the client-side [CLI](https://github.com/strongloop/strongloop) and
[GUI](https://github.com/strongloop/strong-arc) (`slc arc`):

    npm install -g strongloop
    slc -h

Run app

    slc start app.js
    
Or to deploy and manage remotely, install the manager on a production server using npm:

    npm install -g strong-pm && sl-pm-install

Or using docker:

    curl -sO http://strong-pm.io/docker.sh | sudo /bin/sh

## Docs & Community

- [StrongLoop Documentation](http://docs.strongloop.com/display/SLC/Operating+Node+applications)
- [Professional support](http://strongloop.com/node-js/subscription-plans/)
- [StrongLoop Google Group](https://groups.google.com/forum/#!forum/strongloop)
- [GitHub issues](https://github.com/strongloop/strong-pm/issues)
- [Gitter chat](https://gitter.im/strongloop/chat)
- [Install on production server](./INSTALL.md)
- [Application life-cycle](./LIFE-CYCLE.md)
- ***XXX pointers to our blogs***


## Quick Start

Under production, you will install the process manager as a system service, see
http://strong-pm.io/prod, but if you are just trying the manager out locally,
you can run an app directly from the command line.

Get a sample app (or use your own app):

    git clone git@github.com:strongloop/express-example-app.git
    cd express-example-app
    npm install

Start the app under the process manager:

    slc start

Interact with the app using the StrongLoop GUI:

    slc arc

See http://strong-pm.io for more information.


## License

strong-pm uses a dual license model.

You may use this library under the terms of the [Artistic 2.0 license][],
or under the terms of the [StrongLoop Subscription Agreement][].

[Artistic 2.0 license]: http://opensource.org/licenses/Artistic-2.0
[StrongLoop Subscription Agreement]: http://strongloop.com/license
