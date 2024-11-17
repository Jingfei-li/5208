# 5208 project guidance

# Running Spark Project on Google Cloud Storage

This guide will walk you through running a PySpark project to read data from Google Cloud Storage (GCS). The script prepares data by extracting specific fields from a CSV file stored in GCS and processes it using Spark's DataFrame API.

## Prerequisites

Before you start, ensure you have the following:

1. **Google Cloud Platform (GCP)**: A Google Cloud project with billing enabled.
2. **Apache Spark**: Set up on Google Dataproc.
3. **Python Script**: Ensure you have the Python jupyter notebook script (`DSA5208Project2.ipynb`) to be used.
4. **Data**: The data file (`2023.tar.gz`) must be available in your GCS bucket (`gs://<your_bucket_name>/2023.tar.gz`). file (`2023.tar.gz`)  is download from Canvas

## Step 1: Set Up Google Cloud and Dataproc

- Make sure Google Cloud environment is properly set up. You can see the [Google Cloud Setup Guide](https://cloud.google.com/docs/overview).
- Create a Dataproc cluster to run Spark:

  ```sh
  gcloud dataproc clusters create my-cluster \
      --region <your-region> \
      --num-workers 2 \
      --image-version 2.0-debian10 \
      --scopes "cloud-platform"
