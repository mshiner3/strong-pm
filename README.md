# strong-pm - Process Manager

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/strongloop/chat?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

StrongLoop's process manager, strong-pm, allows you to build, deploy, monitor,
and debug your apps in development and in production.

For more details, see http://strong-pm.io.

## Installation

Install the manager on a production server:

    npm install -g strong-pm && sl-pm-install

Or using docker:

    curl -sO https://cdn.rawgit.com/strongloop/strong-pm/master/docker/install.sh | sudo /bin/sh

XXX ^--- above should be a strong-pm.io link

Install the client-side [CLI](https://github.com/strongloop/strongloop) and
[GUI](https://github.com/strongloop/strong-arc) (`slc arc`):

    npm install -g strongloop
    slc -h


## Features

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


## Quick Start

Under production, you will install the process manager as a system service, see
below, but if you are just trying the manager out on localy, you can run an app
directly from the command line.

Get a sample app (or use your own app):

    git clone git@github.com:strongloop/loopback-example-app.git
    cd loopback-example-app
    npm install

XXX use another sample?

Start the app under the process manager:

    slc start

Interact with the app using the StrongLoop GUI:

    slc arc

See [strong-pm.io](http://strong-pm.io) for more information.


## Life-cycle

The process manager is responsible for receiving packaged applications,
preparing them to be run, and then running them under supervision (clustered,
with run-time profiling and metrics support, restart on failure, etc.).

The prepare commands used are:

- `npm rebuild`
- `npm install --production`

Since `npm install` is called, the preparation may be customized using npm
install and pre-install scripts, if necessary.

After preparation, the application is run.


## Installing via npm as a Service

The process manager should be installed as a service, under the control of the
system process manager. This will ensure it is started on machine boot, logs are
correctly aggregated, permissions are set correctly, etc.

The sl-pm-install tool does this installation for you, and supports the
following init systems:

- Upstart 0.6
- Upstart 1.4 (default)
- Systemd

In it's typical usage, you would install the `strong-pm` package globally on
the deployment system and then install it as a service:

    npm install -g strong-pm
    sl-pm-install

It will create a strong-pm user account with `/var/lib/strong-pm` set as its
home directory. If deploying to a hosted service, there may already be a user
account prepared that you want the manager to run as, you can specify it with
the `--user` option.

You can also `--job-file` to generate the service file locally, and move it to
the remote system manually.


## Installing via Docker Hub

This repository is the source of the
[strongloop/strong-pm](https://registry.hub.docker.com/u/strongloop/strong-pm/)
repo on Docker Hub. You can get started as quickly as:

    docker pull strongloop/strong-pm
    docker run -d -p 8701:8701 -p 80:3000 --name strong-pm strongloop/strong-pm

And now you've got a strong-pm container up and running. You can deploy to it
with `slc deploy http://localhost:8701`.

This image can be run as an OS service without the need to install strong-pm,
npm, or even node on your server. All you need is Docker and curl!

    curl -sO https://cdn.rawgit.com/strongloop/strong-pm/master/docker/install.sh | sudo /bin/sh

The created service will use port 8701 for strong-pm's API, port 3000 for your
app, and the container will be restarted if your server reboots.

If you want to step through all the steps yourself, the script is based off of
a guide in [docker/README.md](docker/README.md).

For more information on Docker and Docker Hub, see https://www.docker.com/


## Resources

- [Documentation](http://docs.strongloop.com/display/SLC/Operating+Node+applications).
- [StrongLoop Google Group](https://groups.google.com/forum/#!forum/strongloop).
- [GitHub issues](https://github.com/strongloop/strong-pm/issues).
- [Gitter chat](https://gitter.im/strongloop/chat).


## Usage

### slc start

```
usage: slc start [app/]

Options:
  -h,--help         Print this message and exit.
  -v,--version      Print version and exit.

Start `app` in the background under the process manager.

If not specified, the application in the current working directory is started.
```

### slc pm

```
usage: slc pm [options]

The Strongloop process manager.

Options:
  -h,--help         Print this message and exit.
  -v,--version      Print version and exit.
  -b,--base BASE    Base directory to work in (default `.strong-pm`).
  -l,--listen PORT  Listen on PORT for git pushes (default 8701).
  -C,--control CTL  Listen for local control messages on CTL (default `pmctl`).
  --no-control      Do not listen for local control messages.

The base directory is used to save deployed applications, for working
directories, and for any other files the process manager needs to create.

The process manager will be controllable via HTTP on the port specified. That
port is also used for deployment with strong-deploy. Basic authentication
can be enabled for HTTP by setting the STRONGLOOP_PM_HTTP_AUTH environment
variable to <user>:<pass> (eg. strong-pm:super-secret). This is the same format
as used by the --http-auth option of pm-install.

It is also controllable using local domain sockets, which look like file paths,
and the listen path can be changed or disabled. These sockets do not support
HTTP authentication.
```

### slc ctl

```
usage: slc ctl [options] [command]

Run-time control of the Strongloop process manager.

Options:
  -h,--help               Print help and exit.
  -v,--version            Print version and exit.
  -C,--control CTL        Control endpoint for process manager.

The control endpoint for the process manager is searched for if not specified,
in this order:

1. `STRONGLOOP_PM` in environment: may be a local domain path, or an HTTP URL.
2. `./pmctl`: a process manager running in the current working directory.
3. `~/.strong-pm/pmctl`: a process manager running in the user's home directory.
4. `/var/lib/strong-pm/pmctl`: a process manager installed by pm-install.
5. `http://localhost:8701`: a process manager running on localhost

An HTTP URL is mandatory for remote process managers, but can also be used on
localhost. If the process manager is using HTTP authentication
then valid credentials must be set in the URL directly, such as
`http://user-here:pass-here@example.com:7654`.

When using an HTTP URL, it can optionally be tunneled over ssh by changing the
protocol to `http+ssh://`. The ssh username will default to your current user
and authentication defaults to using your current ssh-agent. The username can be
overridden by setting an `SSH_USER` environment variable. The authentication can
be overridden to use an existing private key instead of an agent by setting the
`SSH_KEY` environment variable to the path of the private key to be used.

Commands:
  status                  Report status, the default command.
  shutdown                Stop the process manager.
  start                   Start the current application.
  stop                    Hard stop the current application.
  soft-stop               Soft stop the current application.
  restart                 Hard stop and restart the current application with
                            new config.
  soft-restart            Soft stop and restart the current application with
                            new config.
        "Soft" stops notify workers they are being disconnected, and give them
        a grace period for any existing connections to finish. "Hard" stops
        kill the supervisor and its workers with `SIGTERM`.

  cluster-restart         Restart the current application cluster workers.
        This is a zero-downtime restart, the workers are soft restarted
        one-by-one, so that some workers will always be available to service
        requests.

  set-size N              Set cluster size to N workers.
        The default cluster size is the number of CPU cores.

  objects-start ID        Start tracking objects on worker ID.
  objects-stop ID         Stop tracking objects on worker ID.
        Object tracking is published as metrics, and requires configuration so
        that the `--metrics=URL` option is passed to the runner.

  cpu-start ID [TIMEOUT]  Start CPU profiling on worker ID.
        TIMEOUT is the optional watchdog timeout, in milliseconds.  In watchdog
        mode, the profiler is suspended until an event loop stall is detected;
        i.e. when a script is running for too long.  Only supported on Linux.

  cpu-stop ID [NAME]      Stop CPU profiling on worker ID.
        The profile is saved as `<NAME>.cpuprofile`. CPU profiles must be
        loaded into Chrome Dev Tools. The NAME is optional, and defaults to
        `node.<PID>`.

  heap-snapshot ID [NAME] Save heap snapshot for worker ID.
        The snapshot is saved as `<NAME>.heapsnapshot`.  Heap snapshots must be
        loaded into Chrome Dev Tools. The NAME is optional, and defaults to
        `node.<PID>`.

  ls [DEPTH]              List dependencies of the current application.

  env[-get] [KEYS...]     List specified environment variables. If none are
                          specified, list all variables.
  env-set K=V...          Set one or more environment variables.
  env-unset KEYS...       Unset one or more environment variables.
        The environment variables are applied to the current application, and
        the application is hard restarted with the new environment after change
        (either set or unset).

  log-dump [--follow]     Empty the log buffer, dumping the contents to stdout.
                          If --follow is given the log buffer is continuously
                          dumped to stdout.

Worker `ID` is either a node cluster worker ID, or an operating system process
ID. The special worker ID `0` can be used to identify the master.
```

### sl-pm-install

This tool is also available as `slc pm-install` if the
[strongloop](https://github.com/strongloop/strongloop) client toolset is
installed.

```
usage: sl-pm-install [options]

Install the Strongloop process manager as a service.

Options:
  -h,--help           Print this message and exit.
  -v,--version        Print version and exit.
  -m,--metrics STATS  Specify --metrics option for supervisor running deployed
                      applications.
  -b,--base BASE      Base directory to work in (default is $HOME of the user
                      that manager is run as, see --user).
  -e,--set-env K=V... Initial application environment variables. If setting
                      multiple variables they must be quoted into a single
                      argument: "K1=V1 K2=V2 K3=V3".
  -u,--user USER      User to run manager as (default is strong-pm).
  -p,--port PORT      Listen on PORT for application deployment (default 8701).
  -n,--dry-run        Don't write any files.
  -j,--job-file FILE  Path of Upstart job to create (default is
                      `/etc/init/strong-pm.conf`).
  -f,--force          Overwrite existing job file if present.
  --upstart VERSION   Specify Upstart version, 1.4 or 0.6 (default is 1.4).
  --systemd           Install as a systemd service, not an Upstart job.
  --http-auth CREDS   Enable HTTP authentication using Basic auth, requiring
                      the specified credentials for every request sent to the
                      REST API where CREDS is given in the form of
                      `<user>:<pass>`.

OS Service support:

The --systemd and --upstart VERSION options are mutually exclusive.  If neither
is specified, the service is installed as an Upstart job using a template that
assumes Upstart 1.4 or higher.
```

The URL formats supported by `--metrics STATS` are defined by strong-supervisor.


## License

strong-pm uses a dual license model.

You may use this library under the terms of the [Artistic 2.0 license][],
or under the terms of the [StrongLoop Subscription Agreement][].

[Artistic 2.0 license]: http://opensource.org/licenses/Artistic-2.0
[StrongLoop Subscription Agreement]: http://strongloop.com/license
