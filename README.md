# WhipperSnapPy

WhipperSnapPY is a small Python OpenGL program to render FreeSurfer and 
FastSurfer surface models and color overlays and generate screen shots.

## Contents:

- Capture 4x4 surface plots (front & back, left and right)
- OpenGL window for interactive visualization

Note, that currently no off-screen rendering is supported. Even in snap 
mode an invisible window will be created to render the openGL output
and capture the contents to an image. In order to run this on a headless
server, inside Docker, or via ssh we recommend to install xvfb and run

```
apt update && apt install -y python3 python3-pip xvfb
pip3 install pyopengl glfw pillow numpy pyrr
xvfb-run python3 whipersnap.py ...
```

## Installation:

The `WhipperSnapPy` package can be installed from this repository using:
```
python3 -m pip install .
```

## Usage:

### Local:

After installing the Python package, the whippersnap program can be run using
the installed command line tool such as in the following example:
```
whippersnap -lh $OVERLAY_DIR/$LH_OVERLAY_FILE \
               -rh $OVERLAY_DIR/$RH_OVERLAY_FILE \
               -sd $SURF_SUBJECT_DIR \
               -o $OUTPUT_DIR/whippersnappy_image.png \
```

Note that adding the `--interactive` flag will start an interactive GUI that
includes a visualization of one hemisphere side and a simple application through
which color threshold values can be configured.

### Docker:

the whippersnap program can be run within a docker container to capture
a snapshot by building the provided Docker image and running a container as
follows:
```
docker build --rm=true -t whippersnappy -f ./Dockerfile .
```
```
docker run --rm --init --name my_whippersnappy -v $SURF_SUBJECT_DIR:/surf_subject_dir \
                                               -v $OVERLAY_DIR:/overlay_dir \
                                               -v $OUTPUT_DIR:/output_dir \
                                               --user $(id -u):$(id -g) whippersnappy:latest \
                                               --lh_overlay /overlay_dir/$LH_OVERLAY_FILE \
                                               --rh_overlay /overlay_dir/$RH_OVERLAY_FILE \
                                               --sdir /surf_subject_dir \
                                               --output_path /output_dir/whippersnappy_image.png
```

In this example: `$SURF_SUBJECT_DIR` contains the surface files, `$OVERLAY_DIR` contains the overlays to be loaded on to the surfaces, `$OUTPUT_DIR` is the local output directory in which the snapshot will be saved, and `${LH/RH}_OVERLAY_FILE` point to the specific overlay files to load.

**Note:** The `--init` flag is needed for the `xvfb-run` tool to be used correctly.

## Links:

We also invite you to check out our lab webpage at https://deep-mi.org
