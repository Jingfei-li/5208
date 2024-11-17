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
9. **Data**: The data file (`2023.tar.gz`) must be available in your GCS bucket (`gs://<your_bucket_name>/2023.tar.gz`). The zip file is download from canvas

   
## Step 1: Set Up Google Cloud and Dataproc

- Make sure Google Cloud environment is properly set up. You can also see the [Google Cloud Setup Guide](https://cloud.google.com/docs/overview).
- Create a Dataproc cluster to run Spark:

  ```sh
   $ gcloud compute networks subnets update default \
   --region=[REGION] --enable-private-ip-google-access
   $ gcloud dataproc clusters create [CLUSTER NAME] \
   --region [REGION] --image-version [IMAGE VERSION] \
   --master-machine-type [MASTER MACHINE TYPE] \
   --master-boot-disk-type [MASTER DISK TYPE] --master-boot-disk-size [DISK SIZE] \
   --num-workers [NUMBER OF WORKERS] \
   --worker-machine-type [WORKER MACHINE TYPE] \
   --worker-boot-disk-type [WORKER DISK TYPE] --worker-boot-disk-size [DISK SIZE] \
   --enable-component-gateway --optional-components JUPYTER
- Alternatively, you can directly create a cluster via the web interface: follow the instructions in Menu ≡ > Dataproc > Clusters > [Cluster Name] > WEB INTERFACES

## Step 2: Upload Files to Google Cloud Storage

Upload Data File: Use the (`gsutil command`) to upload the (`2023.tar.gz`) file to your GCS bucket:
 ```sh
$ gsutil cp path/to/2023.tar.gz gs://<your_bucket_name>/
   ```
Upload Jupyter Notebook: Similarly, upload the Jupyter notebook (`DSA5208Project2.ipynb`) to your GCS bucket:
 ```sh
$ gsutil cp path/to/DSA5208Project2.ipynb gs://<your_bucket_name>/
   ```
## Step 3: Run the Jupyter Notebook
1. **Access Jupyter**:Once your cluster is running, access the Jupyter Notebook interface from the Google Cloud Console. Navigate to Dataproc > Clusters > [Cluster Name] and click WEB INTERFACES to find the Jupyter link.
   
2. **Run the Notebook**:Once inside Jupyter, open the uploaded notebook (`DSA5208Project2.ipynb`)
-  Adjust the paths in the notebook to point to the data in GCS (`gs://<your_bucket_name>/2023.tar.gz`).
-  Execute each cell to run the analysis.

## Step 4: Submit a PySpark Job
To run the notebook as a Spark job, you can use the following command to submit it to your Dataproc cluster:
 ```sh
$ gcloud dataproc jobs submit pyspark gs://<your_bucket_name>/DSA5208Project2.ipynb \
--cluster [CLUSTER NAME] \
--region [REGION]
 ```

## Step 5: Verify results
- After running the notebook or submitting the PySpark job, you can do the following to verify:
- **View Data**: Use commands like df.show(5) to display the first five rows of the processed data.
- **Monitor Jobs**: Monitor your Spark job’s progress in the Google Cloud Console under Dataproc > Jobs.

## Step 6: Clean Up
- **Stop the Cluster**: To avoid incurring costs, stop or delete the Dataproc cluster after completing your work:
 ```sh
$ gcloud dataproc clusters delete [CLUSTER NAME] --region [REGION]
 ```
- **Delete GCS Files**: If you no longer need the data or notebook, delete them from the GCS bucket
 ```sh
$ gsutil rm gs://<your_bucket_name>/2023.tar.gz
$ gsutil rm gs://<your_bucket_name>/DSA5208Project2.ipynb
 ```
## Additional Notes
Ensure the input file in GCS (`2023.tar.gz`) is correctly formatted for the script to work properly.
Adjust the Spark configurations if running on a cluster with limited resources for optimal performance.

## Troubleshooting
- File Not Found Error: Ensure that the GCS path (`gs://<your_bucket_name>/2023.tar.gz`) is correct and accessible.
- Permission Denied: Verify that your service account has the correct permissions to access GCS and Dataproc resources.

## Resources
- Google Cloud Documentation
- Apache Spark Documentation
