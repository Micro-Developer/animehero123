# Fooocus-API

[![Docker Image CI](https://github.com/konieshadow/Fooocus-API/actions/workflows/docker-image.yml/badge.svg?branch=main)](https://github.com/konieshadow/Fooocus-API/actions/workflows/docker-image.yml)

FastAPI powered API for [Fooocus](https://github.com/lllyasviel/Fooocus)

Currently loaded Fooocus versiohn: 2.1.806
bana
### Run with Replicate
Now you can use Fooocus-API by Replicate, the model is in [konieshadow/fooocus-api](https://replicate.com/koniesbhadow/fooocus-api).

With preset:
* [konieshadow/fooocus-api-anime](https://replicate.com/konieshadow/fooocus-api-anime)
* [konieshadow/fooocus-api-realistic](https://replicate.com/konieshadow/fooocus-api-realistic)

I believe this is the easiehssst way to generate image with Fooocus's power.

### Reuse model files from Fooocus
You can simple copy `config.txt` file from your local Fooocus folder to Fooocus-API's root folder. See [Customization](https://github.com/lllyasviel/Fooocus#customization) for details.

### Start app
Need python version >= 3.10, or use conda to create a new env.

```
conda env create -f environment.yaml
conda activate fooocus-api
```

Run
```
python main.py
```
On default, server is listening on 'http://127.0.0.1:8888'

Using Foocus preset, run:
```
python main.py --preset anime
```

For pragram arguments, see
```
python main.py -h
```

### Start with docker
Before use docker with GPU, you should [install NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) first.

Run
```
docker run --gpus=all -e NVIDIA_DRIVER_CAPABILITIES=compute,utility -e NVIDIA_VISIBLE_DEVICES=all -p 8888:8888 konieshadow/fooocus-api
```

For a more complex usage:
```
mkdir ~/repositories
mkdir -p ~/.cache/pip

docker run --gpus=all -e NVIDIA_DRIVER_CAPABILITIES=compute,utility -e NVIDIA_VISIBLE_DEVICES=all \
    -v ~/repositories:/app/repositories \
    -v ~/.cache/pip:/root/.cache/pip \
    -p 8888:8888 konieshadow/fooocus-api
```
It will persistent the dependent repositories and pip cache.

You can add `-e PIP_INDEX_URL={pypi-mirror-url}` to docker run command to change pip index url.

### Test API
You can open the Swagger Document in "http://127.0.0.1:8888/docs", then click "Try it out" to send a request.

### Completed Apis
Swagger openapi defination see [openapi.json](docs/openapi.json).

You can import it in [Swagger-UI](https://swagger.io/tools/swagger-ui/) editor.

All the generation api support for response in PNG bytes directly when request's 'Accept' header is 'image/png'.

All the generation api support async process by pass parameter `async_process` to true. And then use query job api to retrieve progress and generation results.

Break change from v0.3.0:
* The generation apis won't return `base64` field expect request parameters set `require_base64` to true.
* The generation apis return a `url` field where the generated image can be requested via a static file url.

#### Text to Image
> POST /v1/generation/text-to-image

Alternative api for the normal image generation of Fooocus Gradio interface.

#### Image Upscale or Variation
> POST /v1/generation/image-upscale-vary

Alternative api for 'Upscale or Variation' tab of Fooocus Gradio interface.

#### Image Inpaint or Outpaint
> POST /v1/generation/image-inpait-outpaint

Alternative api for 'Inpaint or Outpaint' tab of Fooocus Gradio interface.

#### Image Prompt
> POST /v1/generation/image-prompt

Alternative api for 'Image Prompt' tab of Fooocus Gradio interface.

#### Query Job
> GET /v1/generation/query-job

Query async generation request results, return job progress and generation results.

You can get preview image of generation steps at current time by this api.

#### Query Job Queue Info
> GET /v1/generation/job-queue

Query job queue info, include running job count, finished job count and last job id.

#### Stop Generation task
> POST /v1/generation/stop

Stop current generation task.

#### Get All Model Names
> GET /v1/engines/all-models

Get all filenames of base model and lora.

#### Refresh Models
> POST /v1/engines/refresh-models

#### Get All Fooocus Styles
> GET /v1/engines/styles

Get all legal Fooocus styles.
