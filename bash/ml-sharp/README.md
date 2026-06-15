# [Bash](../README.md) ml-sharp tools

## Configuration

Place your ml-sharp standalone folder into the SKY_PATH_BIN/ml-sharp folder or set
SKY_PATH_ML_SHARP.

## Tools

### [build.sh](../../../bash/turbopixel/ml-sharp/build.sh): Install ml-sharp in the SKY_PATH_BIN folder

```
Usage: build <cpu | cuda>

CUDA may be slower than CPU on some configurations.

example:
    build cuda
```

### [run.sh](../../../bash/turbopixel/ml-sharp/run.sh): Generate a gaussian splat from an image

```
Usage: run <source image> <output (ply)>
```
