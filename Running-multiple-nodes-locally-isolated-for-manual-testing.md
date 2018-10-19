An easy way to run multiple nodes locally for testing:


* Choose a unique port for each node you wish to run.  e.g. 3 nodes with ports `6001`, `6002`, `6003`

* Create a file `localhost-peers.txt`:

```
127.0.0.1:6001
127.0.0.1:6002
127.0.0.1:6003
```

* Open a terminal shell for each node you wish to run.

* Use the following invocation for each node:

```sh
PORT=6001 ./run-client.sh -localhost-only -custom-peers-file=localhost-peers.txt -download-peerlist=false -launch-browser=false -data-dir=/tmp/$PORT -web-interface-port=$(expr $PORT + 420) -port=$PORT
```

Replace `PORT=` with the correct port number.

This will run a node on `$PORT` with its own `data-dir` of `/tmp/$PORT` and expose the web API on `$PORT + 420` (e.g. `6421`).