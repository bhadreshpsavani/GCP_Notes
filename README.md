# GCP_Notes

## Project and region setting

* project  setting: `export PROJECT_ID=$(gcloud config get-value project)`
* Region setting:
  ```cmd
  export REGION=us-west1
  gcloud config set compute/region $REGION
  ```

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
