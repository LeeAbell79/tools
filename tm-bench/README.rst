Benchmarking and Monitoring
===========================

tm-bench
--------

Tendermint blockchain benchmarking tool: https://github.com/tendermint/tools/tree/master/tm-bench

For example, the following:

::

    tm-bench -T 10 -r 1000 localhost:46657

will output:

::

    Stats             Avg        Stdev      Max
    Block latency     6.18ms     3.19ms     14ms
    Blocks/sec        0.828      0.378      1
    Txs/sec           963        493        1811

Quick Start
^^^^^^^^^^^

Docker
~~~~~~

::

    docker run -it --rm -v "/tmp:/tendermint" tendermint/tendermint init
    docker run -it --rm -v "/tmp:/tendermint" -p "46657:46657" --name=tm tendermint/tendermint

    docker run -it --rm --link=tm tendermint/bench tm:46657

Binaries
~~~~~~~~

If **Linux**, start with:

::

    curl -L https://s3-us-west-2.amazonaws.com/tendermint/0.10.4/tendermint_linux_amd64.zip && sudo unzip -d /usr/local/bin tendermint_linux_amd64.zip && sudo chmod +x tendermint

if  **Mac OS**, start with:

::

    curl -L https://s3-us-west-2.amazonaws.com/tendermint/0.10.4/tendermint_darwin_amd64.zip && sudo unzip -d /usr/local/bin tendermint_darwin_amd64.zip && sudo chmod +x tendermint

then run:

::

    tendermint init
    tendermint node --app_proxy=dummy

    tm-bench localhost:46657

with the last command being in a seperate window.

Usage
^^^^^

::

    tm-bench [-c 1] [-T 10] [-r 1000] [endpoints]

    Examples:
            tm-bench localhost:46657
    Flags:
      -T int
            Exit after the specified amount of time in seconds (default 10)
      -c int
            Connections to keep open per endpoint (default 1)
      -r int
            Txs per second to send in a connection (default 1000)
      -v    Verbose output

tm-monitor
----------

Tendermint blockchain monitoring tool; watches over one or more nodes, collecting and providing various statistics to the user: https://github.com/tendermint/tools/tree/master/tm-monitor

Quick Start
^^^^^^^^^^^

Docker
~~~~~~

::

    docker run -it --rm -v "/tmp:/tendermint" tendermint/tendermint init
    docker run -it --rm -v "/tmp:/tendermint" -p "46657:46657" --name=tm tendermint/tendermint

    docker run -it --rm --link=tm tendermint/monitor tm:46657

Binaries
~~~~~~~~

This will be the same as you did for ``tm-bench`` above, except for the last line which should be:

::

    tm-monitor localhost:46657

Usage
^^^^^

::

    tm-monitor [-v] [-no-ton] [-listen-addr="tcp://0.0.0.0:46670"] [endpoints]

    Examples:
            # monitor single instance
            tm-monitor localhost:46657

            # monitor a few instances by providing comma-separated list of RPC endpoints
            tm-monitor host1:46657,host2:46657
    Flags:
      -listen-addr string
            HTTP and Websocket server listen address (default "tcp://0.0.0.0:46670")
      -no-ton
            Do not show ton (table of nodes)
      -v    verbose logging

RPC UI
^^^^^^

Run ``tm-monitor`` and visit http://localhost:46670
You should see the list of the available RPC endpoints:

::

    http://localhost:46670/status
    http://localhost:46670/status/network
    http://localhost:46670/monitor?endpoint=_
    http://localhost:46670/status/node?name=_
    http://localhost:46670/unmonitor?endpoint=_

The API is available as GET requests with URI encoded parameters, or as JSONRPC
POST requests. The JSONRPC methods are also exposed over websocket.

