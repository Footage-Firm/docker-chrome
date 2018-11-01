### Start the Chrome docker container

docker run --rm -it -p 5900:5900 -p 22:22 --name chrome --user apps --privileged videoblocks/chrome

### SSH Workaround

Chrome currently does not support "remote-debugging-address" in headless mode, so for non-headless mode within a docker container we must run an ssh server within the chrome container and use ssh port-forwarding.

```
user@localhost$ docker run --rm -it -p 5900:5900 -p 22:22 --name chrome --privileged videoblocks/chrome

apps@container$ google-chrome --remote-debugging-port=9222 --remote-debugging-address=0.0.0.0 --no-sandbox

user@localhost$ ssh -L 0.0.0.0:9222:localhost:9222 root@0.0.0.0 -N
user@localhost$ curl 0.0.0.0:9222/json # Should give response containing "webSocketDebuggerUrl"
```

### Connect with VNC Viewer

0.0.0.0:5900

### Launch chrome within the docker container and try to curl the devtools server.

```
apps@container$ google-chrome --remote-debugging-port=9222 --remote-debugging-address=0.0.0.0 --headless
user@localhost$ curl 0.0.0.0:9222/json # Works!

apps@container$ google-chrome --remote-debugging-port=9222 --remote-debugging-address=0.0.0.0
user@localhost$ curl 0.0.0.0:9222/json # Cannot connect from host container :(
```

## TODO:

- [ ] Launch chrome from Dockerfile.
- [ ] Use supervisord or another method for forking processes.
- [ ] Configure puppeteer to connect to the container running Chrome via websocket (curl _/json_ then get "webSocketDebuggerUrl").
