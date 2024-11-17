# 5208 project guidance

# Running Spark Project on Google Cloud Storage

This guide will walk you through running a PySpark project to read data from Google Cloud Storage (GCS). The script prepares data by extracting specific fields from a CSV file stored in GCS and processes it using Spark's DataFrame API.

## Prerequisites

Before you start, ensure you have the following:
### Set Up Google Cloud Environment

1. **Log in to your Google account on terminal**:
   ```sh
   $ gcloud auth login
   ```
2. **Create a project**:
   ```sh
   $ gcloud projects create [PROJECT NAME]
   $ gcloud config set project [PROJECT NAME]
   ```
3. **Link to a billing account**:
   ```sh
    $ gcloud billing accounts list
    $ gcloud billing projects link [PROJECT NAME] --billing-account=[ACCOUNT ID] 
   ```
4. **Enable Google APIs**:
   ```sh
    $ gcloud services enable dataproc.googleapis.com
    $ gcloud services enable cloudresourcemanager.googleapis.com
   ```
5. **Set up permissions**:
   ```sh
    $ gcloud iam service-accounts list
    $ gcloud projects add-iam-policy-binding [PROJECT NAME] \
    --member="serviceAccount:[EMAIL]" --role="roles/dataproc.worker"
   ```

6. **Hadoop and HDFS**: A working Hadoop cluster with HDFS set up.
7. **Apache Spark**: Set up on Google Dataproc.
8. **Python Script**: Ensure you have the jupyter notebook script (`DSA5208Project2.ipynb`) to be used.
9. **Data**: The data file (`2023.tar.gz`) must be available in your GCS bucket (`gs://<your_bucket_name>/2023.tar.gz`).

   
## Step 1: Set Up Google Cloud and Dataproc

- Make sure Google Cloud environment is properly set up. You can also see the [Google Cloud Setup Guide](https://cloud.google.com/docs/overview).
- Create a Dataproc cluster to run Spark:

  ```sh
  gcloud dataproc clusters create my-cluster \
      --region <your-region> \
      --num-workers 2 \
      --image-version 2.0-debian10 \
      --scopes "cloud-platform"
- Or can directly create cluster on web page follow by the instructions
