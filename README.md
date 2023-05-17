
## Azure portal

Size:
Standard NC4as T4 v3 (4 vcpus, 28 GiB memory)

VMI: 
nvidia-gpu-optimized-vmi-a10

## SSH to VM

sudo apt update

sudo apt upgrade

conda create --name md --python=3.9.16

conda activate md

git clone https://github.com/larry-wps/megadetector-fastapi.git
cd megadetector-fastapi

pip install -r requirements/requirements-dev.txt

**To run just the API:**
cd api
uvicorn main:app --host 0.0.0.0 --port 8000 --reload

**To run just the UI**
cd ../ui/
streamlit run app.py

**To run both**
start.sh


**Note**: Streamlit reads the FastAPI server location from an environment variable `MEGADETECTOR_API_URL` and defaults to `http://127.0.0.1:8000/` if it is missing. If the server port is different, you can directly update it in the `config.py` file or put it in a `.env` file (like this `MEGADETECTOR_API_URL='http://127.0.0.1:8000'`) which might be preferrable if the URL is public and shouldn't be on git.

## Docker stuff

There is a docker template to containerize the application and deploy it. Currently, the configuration is tailored to create containers that run on CPUs. This is primarily for Cloud based APIs, specifically, Google Cloud Run (soon to be tested).

Deployment strategies like Google Cloud Run spin up instances on demand. In this setting, instead of downloading the MegaDetector model everytime,  it might be useful to create images with the MegaDetector models baked in the container images. However, to keep them light, it is best to only have a specific model in addition to its required dependencies installed (PyTorch & Tensorflow).

