# GCP_Notes

## Project and region setting

* project  setting: `export PROJECT_ID=$(gcloud config get-value project)`
* Region setting:
  ```
  export REGION=us-west1
  gcloud config set compute/region $REGION
  ```

## Dataplex

Create a lake in dataplex
```
gcloud dataplex lakes create ecommerce \
   --location=$REGION \
   --display-name="Ecommerce" \
   --description="Ecommerce Domain"
```
