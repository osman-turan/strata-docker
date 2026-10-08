# Strata Docker image

This repository builds the NVIDIA CUDA Docker image from the latest published [Strata release](https://github.com/Niko1221/Strata/releases) and publishes it to Docker Hub as [`osmanturan/strata`](https://hub.docker.com/r/osmanturan/strata). It checks daily for a new upstream release and records the source release and commit in OCI labels. The build uses Strata's Dockerfile and entrypoint unchanged, including the defaults supplied by that release.

## Image tags

Each CUDA build is published with four tags: `latest`, the upstream release tag (for example, `v0.1.41`), `cuda-latest`, and a backend-qualified release tag (for example, `cuda-v0.1.41`). All four tags point to the same image build. Docker uses `latest` when a pull or run command omits the tag, so `latest` remains the CUDA default and keeps existing image references working. Use `cuda-latest` or `cuda-v0.1.41` when you want the backend to be explicit.

## Use on Unraid

Install the NVIDIA Docker support and driver required by the Strata release you plan to run. Check that release's Docker instructions for current requirements. Configure the container with `osmanturan/strata:latest` (or `osmanturan/strata:cuda-latest`), map host port `8080` to container port `8080`, and map `/mnt/user/appdata/strata` to `/data` for persistent model data. Set the Extra Parameters to `--runtime=nvidia`, `NVIDIA_VISIBLE_DEVICES` to `all` and `NVIDIA_DRIVER_CAPABILITIES` to `all`.

The image leaves Strata's defaults in place, so model and setup defaults come from the upstream release and can change with it. Add Strata environment variables only when you want to override a setting; consult the upstream [Docker instructions](https://github.com/Niko1221/Strata/blob/main/README.md#docker-linux) and [model guide](https://github.com/Niko1221/Strata/blob/main/docs/MODELS.md) for the release you are using. Add `API_KEY` with a long random value before making the server reachable from other machines. Keep `/mnt/user/appdata/strata` on persistent storage with enough free space for model downloads.

To change a model setup after it has been saved on the `/data` volume, set `REINSTALL=1` for one start.
