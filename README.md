# DetailGen3D-devcontainer

This repository offers a simple way to run DetailGen3D in a Docker environment.

## Requirements
* Docker ([rootless](https://docs.docker.com/engine/security/rootless) is even better)
* [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
* VSCode with the [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension installed

## Usage
1. Pull DetailGen3D submodule:
```bash
git submodule update --init --recursive
```
2. Comment the `snapshot_download()` methods to use your local shared models:
```python
# snapshot_download(repo_id="VAST-AI/DetailGen3D", local_dir=local_dir)
```
3. In the `.devcontainer/devcontainer.json` file, update the mounts configuration with your models data root:
```json
"mounts": [
  "source=${localEnv:HOME}/Path/To/My/Models,target=/home/vscode/DetailGen3D/pretrained_weights,type=bind,readonly"
]
```
4. Navigate to DetailGen3D and run the demo generation:
```bash
cd DetailGen3D && python scripts/inference_detailgen3d.py --mesh_input assets/model/cb7e6c4a-b4dd-483c-9789-3d4887ee7434.glb --image_input assets/image/cb7e6c4a-b4dd-483c-9789-3d4887ee7434.png
```

## Building and running the image manually
```bash
docker build . -t detailgen3d-devcontainer -f .devcontainer/Dockerfile
docker run -it --name detailgen3d --gpus all --mount type=bind,src=.,dst=/home/vscode --mount type=bind,src=$HOME/Path/To/My/Models,dst=/home/vscode/DetailGen3D/pretrained_weights,ro detailgen3d-devcontainer
export FORCE_CUDA=1
pip install diso torch-cluster --no-build-isolation
cd /home/vscode/DetailGen3D && python scripts/inference_detailgen3d.py --mesh_input assets/model/cb7e6c4a-b4dd-483c-9789-3d4887ee7434.glb --image_input assets/image/cb7e6c4a-b4dd-483c-9789-3d4887ee7434.png
```
