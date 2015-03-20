# strong-pm - Process Manager

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/strongloop/chat?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

StrongLoop's process manager, strong-pm, allows you to build, deploy, monitor,
and debug your apps in development and in production.

For more details, see http://strong-pm.io.


## Installation

***XXX @ijroth - remove this section? I think its useful, maybe move after
Features and before Quick Start***

Install the client-side [CLI](https://github.com/strongloop/strongloop) and
[GUI](https://github.com/strongloop/strong-arc) (`slc arc`):

    npm install -g strongloop
    slc -h

Install the manager on a production server using npm:

    npm install -g strong-pm && sl-pm-install

Or using docker:

    curl -sO http://strong-pm.io/docker.sh | sudo /bin/sh


## Features

***XXX original short version***

- full life-cycle toolset, most tools available as both CLI and GUI
- git and http-based deploy
- strong authentication and encryption via basic auth and ssh
- zero down-time restart on redeploy
- robust multi-cpu usage and restart on failure via node
  [cluster](https://nodejs.org/api/cluster.html#cluster_how_it_works)
- run-time CPU and heap profiling
- log, process environment, and npm dependency managment
- performance metrics, such as loop times, requests response times, etc
- expanding list of 3rdparty integrations, metrics can be published to
  [etsy/statsd](https://github.com/etsy/statsd) compatible servers, such as
  [DataDog](https://www.datadoghq.com/), as well as natively to
  [Graphite](http://graphite.readthedocs.org/en/latest) and
  [Splunk](http://www.splunk.com), and even syslog and raw log files.


## Features

***XXX Long version from https://github.com/strongloop/strong-pm.io/blob/master/index.html***

***XXX Issac: you said you wanted feature list from strong-pm.io, see below, is
this what you meant?***

StrongLoop provides tools for the entire Node devops lifecyle.

- Build, package, and deploy your Node application to a local or remote system.
- View CPU profiles and heap snapshots to optimize performance and diagnose memory leaks.
- Manage processes and clusters using StrongLoop Process Manager.
- View performance metrics on your application.
- Use graphical tool StrongLoop Arc, **slc arc**, or command-line tool, **slc**.

### Build & Deploy

- Automated build and multi-host deploy.
- Zero-downtime application restarts and upgrades.
- Install dependencies, run custom build steps, and prune development dependencies without affecting your source tree.
- Create an npm package or commit the build onto a Git deploy branch.

### Profile

- Heap snapshots and CPU profiles.
- Profile local or remote applications with Arc.
- Profile local applications with slc.
- Use the _watchdog_ to start CPU profiling when the Node event loop stalls.

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
original Feature list***


## Docs & Community

- [StrongLoop Documentation](http://docs.strongloop.com/display/SLC/Operating+Node+applications).
- [Professional support](http://strongloop.com/node-js/subscription-plans/)
- [StrongLoop Google Group](https://groups.google.com/forum/#!forum/strongloop).
- [GitHub issues](https://github.com/strongloop/strong-pm/issues).
- [Gitter chat](https://gitter.im/strongloop/chat).
- [Install on production server](./INSTALL.md)
- [Application life-cycle](./LIFE-CYCLE.md)


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
