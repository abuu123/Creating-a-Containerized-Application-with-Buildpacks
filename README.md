# Creating-a-Containerized-Application-with-Buildpacks
gcloud auth list
gcloud config list project
gcloud config set compute/region $REGION

##Enabled required APIs:
gcloud services enable artifactregistry.googleapis.com run.googleapis.com translate.googleapis.com

$$Downloaded and unzipped the Python sample:
gcloud storage cp gs://cloud-training/CBL513/sample-apps/sample-py-app.zip .
unzip sample-py-app.zip

##Installed pack CLI:
curl -sSL "https://github.com/buildpacks/pack/releases/download/v0.39.0/pack-v0.39.0-linux.tgz" | sudo tar -C /usr/local/bin/ --no-same-owner -xzv

##Built the image using Google’s Buildpacks:
pack build --builder=gcr.io/buildpacks/builder sample-py-app

##Verified the image exists:
docker images

##Ran the container locallydocker run -it -e PORT=8080 -p 8080:8080 -d sample-py-app
curl http://localhost:8080/

##Deployed using Buildpacks directly from source:
gcloud run deploy sample-py-app --source . --region=${REGION} --allow-unauthenticated

##Fetched the deployed service URL:
SERVICE_URL=$(gcloud run services describe sample-py-app --region=${REGION} --format="value(status.url)")
echo $SERVICE_URL

##Tested the deployed service
Called the translation endpoint:
curl $SERVICE_URL -H 'Content-Type: application/json' -d '{"text":"Welcome to this sample app, built with Google Cloud buildpacks."}'


