# geminimulti
gemini streamlit multimodal

# Create Docker
GCP_PROJECT='core-respect-426210-t1'
GCP_REGION='us-central1'
AR_REPO='gemini-repo'
SERVICE_NAME='gemini-streamlit-app' 
gcloud artifacts repositories create "$AR_REPO" --location="$GCP_REGION" --repository-format=Docker
gcloud builds submit --tag "$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/$AR_REPO/$SERVICE_NAME"

# Cloud run deployment
Cloud run deployment

gcloud run deploy "$SERVICE_NAME" \
  --port=8080 \
  --image="$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/$AR_REPO/$SERVICE_NAME" \
  --allow-unauthenticated \
  --region=$GCP_REGION \
  --platform=managed  \
  --project=$GCP_PROJECT \
  --set-env-vars=GCP_PROJECT=$GCP_PROJECT,GCP_REGION=$GCP_REGION

# if i use UI to create this is how it looks like
gcloud run deploy gemini-streamlit-app \
--image=us-central1-docker.pkg.dev/core-respect-426210-t1/gemini-repo/gemini-streamlit-app@sha256:a91d40e071e8548161f64671610c76ed40e13b5099411eb00d6cf159707a64af \
--allow-unauthenticated \
--port=8080 \
--service-account=177522998059-compute@developer.gserviceaccount.com \
--max-instances=10 \
--region=us-central1 \
--project=core-respect-426210-t1
