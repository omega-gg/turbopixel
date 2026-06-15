# [Bash](../README.md) diffusion tools

Shared rendering server that loads the z-image, flux2 and qwen-image pipelines. The generators
offload their work to it through the `[server]` argument of their run scripts, which keeps the
model resident between requests.

## Configuration

Place your diffusion standalone folder into the SKY_PATH_BIN/diffusion folder or set
SKY_PATH_DIFFUSION.

## Tools

### [build.sh](../../../bash/turbopixel/diffusion/build.sh): Install diffusion in the SKY_PATH_BIN folder

```
Usage: build <cpu | cuda | mps | clean>

example:
    build cuda
```

### [check.sh](../../../bash/turbopixel/diffusion/check.sh): Check the install validity

```
Usage: check
```

### [server.sh](../../../bash/turbopixel/diffusion/server.sh): Control the rendering server

```
Usage: server <action> [port = 8080] [scan]

actions:
    start:  start the server
    stop:   stop the server
    cancel: stop the current task
    clear:  stop the current task and release the loaded model

scan: with 'start', bind the first free port in [port, port + 19]

examples:
    server start
    server start  9000
    server start  9000 scan
    server stop   9000
    server cancel 9000
    server clear  9000
```
