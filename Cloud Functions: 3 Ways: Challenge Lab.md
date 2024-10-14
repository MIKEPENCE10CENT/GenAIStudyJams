# Cloud Functions: 3 Ways: Challenge Lab

## Task 1: Create a Cloud Storage Bucket

Create a Cloud Storage bucket in `GIVEN_REGION` using your Project ID as the bucket name.

```bash
export HTTP_FUNCTION=
export FUNCTION_NAME=
export REGION=
export BUCKET=gs://[PROJECT_ID]
```

Enable the necessary services:

```bash
gcloud services enable \
  artifactregistry.googleapis.com \
  cloudfunctions.googleapis.com \
  cloudbuild.googleapis.com \
  eventarc.googleapis.com \
  run.googleapis.com \
  logging.googleapis.com \
  pubsub.googleapis.com
```

Set up the service account and assign the required roles:

```bash
PROJECT_NUMBER=$(gcloud projects list --filter="project_id:$DEVSHELL_PROJECT_ID" --format='value(project_number)')
SERVICE_ACCOUNT=$(gsutil kms serviceaccount -p $PROJECT_NUMBER)
gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID \
  --member serviceAccount:$SERVICE_ACCOUNT \
  --role roles/pubsub.publisher
```

---

## Task 2: Create, Deploy, and Test a Cloud Storage Function (2nd Gen)

1. Create the function directory and files:

    ```bash
    mkdir ~/$FUNCTION_NAME && cd $_
    touch index.js && touch package.json
    ```

2. Deploy the Cloud Function:

    ```bash
    gcloud functions deploy $FUNCTION_NAME \
      --gen2 \
      --runtime nodejs20 \
      --entry-point $FUNCTION_NAME \
      --source . \
      --region $REGION \
      --trigger-bucket $BUCKET \
      --trigger-location $REGION \
      --max-instances 2
    ```

---

## Task 3: Create and Deploy an HTTP Function (2nd Gen) with Minimum Instances

1. Navigate to the parent directory:

    ```bash
    cd ..
    ```

2. Create the HTTP function directory and files:

    ```bash
    mkdir ~/$HTTP_FUNCTION && cd $_
    touch index.js && touch package.json
    ```

3. Deploy the HTTP Cloud Function:

    ```bash
    gcloud functions deploy $HTTP_FUNCTION \
      --gen2 \
      --runtime nodejs20 \
      --entry-point $HTTP_FUNCTION \
      --source . \
      --region $REGION \
      --trigger-http \
      --timeout 600s \
      --max-instances 2 \
      --min-instances 1
    ```
