# GCP_Notes

## Skills

* Storage
* Cloud Function
* Kubeflow
* Kubernates
* Airflow
* Vertex AI
* DataProc
* DaraPrep: An intelligent data service for visually exploring, cleaning, and preparing data for analysis. Cloud Dataprep is serverless and works at any scale. There is no infrastructure to deploy or manage. Easy data preparation with clicks and no code!
* DataFlow
* PubSum
* Bigquery
* Source Repository
* Artifact Store
* Metadata

## Conceptual:
* Google Cloud Run is very similar to App Engine standard environment, except that Cloud Run allows you to bring your own container (BYOC).
  
## Project and region setting

* project  setting: `export PROJECT_ID=$(gcloud config get-value project)`
* Region setting:
  ```cmd
  export REGION=us-west1
  gcloud config set compute/region $REGION
  ```
* Addition of new roles
  ```
  export PROJECT_ID=$(gcloud config get-value project)
  export USER_ACCOUNT=$(gcloud config get-value core/account)
  gcloud projects add-iam-policy-binding $PROJECT_ID --member=user:$USER_ACCOUNT --role=roles/aiplatform.admin
  ```

## VM

* Creating VM
  `gcloud compute instances create gcelab2 --machine-type e2-medium --zone=$ZONE`

* SSH VM
  `gcloud compute ssh gcelab2 --zone=$ZONE`
## Bucket

* Set Retention Policy to the bucket `gsutil retention set 10s "gs://$BUCKET"`
* grant all users read permission for the object stored in your bucket, which will make it publicly available `gsutil acl ch -u AllUsers:R gs://$BUCKET/ada.jpg`

## Bigquery

Create a bigquery dataset: `bq mk --location=$REGION --dataset orders`

## Dataplex

* Create a lake in dataplex
  ```cmd
  gcloud dataplex lakes create ecommerce \
     --location=$REGION \
     --display-name="Ecommerce" \
     --description="Ecommerce Domain"
  ```

* Create a zone within the lake
  ```cmd
  gcloud dataplex zones create orders-curated-zone \
    --location=$REGION \
    --lake=ecommerce \
    --display-name="Orders Curated Zone" \
    --resource-location-type=SINGLE_REGION \
    --type=CURATED \
    --discovery-enabled \
    --discovery-schedule="0 * * * *"
  ```
  
* Attach the BigQuery dataset to the zone
  ```
  gcloud dataplex assets create orders-curated-dataset \
    --location=$REGION \
    --lake=ecommerce \
    --zone=orders-curated-zone \
    --display-name="Orders Curated Dataset" \
    --resource-type=BIGQUERY_DATASET \
    --resource-name=projects/$PROJECT_ID/datasets/orders \
    --discovery-enabled 
  ```

* Create a service account
  ```
  export SA_NAME="document-ai-service-account"
  gcloud iam service-accounts create $SA_NAME --display-name $SA_NAME
  ```
  
* Detech an Asset
  `gcloud dataplex assets delete orders-curated-dataset --location=$REGION --zone=orders-curated-zone --lake=ecommerce`
  
* Delete Zone
  `gcloud dataplex zones delete orders-curated-zone --location=$REGION --lake=ecommerce`
  
* Delete Lake
  `gcloud dataplex lakes delete ecommerce --location=$REGION`
