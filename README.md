# Data-Ingestion-and-Processing-Pipeline

Overview
This repository contains a shell script designed to automate the process of ingesting, validating, and processing data from cloud storage (such as AWS S3) into a Hadoop Data Lake. The script is particularly useful for managing large datasets, ensuring data integrity, and automating the transfer and archiving of data.

Script Description
Script Name: sfm_insuredata.sh
Purpose
The primary purpose of this script is to ingest insurance-related data from a cloud storage location, validate the data, cleanse it, and finally move it to a Hadoop Distributed File System (HDFS) Data Lake. The script also handles error logging, archiving of processed data, and creation of indicator files for downstream data consumers.

Key Features
Data Ingestion: Automates the download of data from cloud storage to a local file system (LFS) and stages it for further processing.
Validation: Validates the data against a predefined schema or expected format. If the data does not meet the validation criteria, it is moved to a rejection folder.
Cleansing: Removes unnecessary trailer information from the data, ensuring that only the required data is processed.
HDFS Data Lake Integration: Moves validated and cleansed data from the LFS to the HDFS Data Lake, organizing it in a structured directory format.
Archival: Compresses and archives the processed data on the local file system after it has been successfully ingested into HDFS.
Error Handling and Logging: Logs all key activities, errors, and decisions made during the execution of the script, facilitating troubleshooting and auditing.

Script Execution
To run the script, use the following command:

bash
Copy code
bash sfm_insuredata.sh <cloud_data_url>
<cloud_data_url>: The URL from which the data is to be fetched (e.g., an S3 bucket URL).
Steps Involved
Initialization:

The script begins by checking if the correct number of arguments is provided.
It initializes variables such as the current date and time, which are used for naming files and directories.
Preparation Work:

Checks and prepares the Landing Pad and Staging directories on the local file system.
Ensures that the archive directory is available, creating it if necessary.
Data Ingestion:

The script downloads the data from the provided cloud storage URL to the local staging area.
Verifies whether the data was successfully downloaded.

Validation & Cleansing:

The script checks if the downloaded file matches the expected record count as indicated in the trailer.
If the validation fails, the data is moved to a rejection folder, and an error is logged.
The trailer line is removed from the file for further processing.
HDFS Data Lake Ingestion:

The script ensures that the target directory in HDFS is ready.
It moves the cleansed data from the local file system to HDFS, creating an indicator file (_SUCCESS) upon successful completion.
Archival:

The processed file is compressed and moved to the archive directory on the local file system.

Completion:

Logs the completion of the data pipeline process.
Exits the script with appropriate status codes depending on success or failure.
Prerequisites
Hadoop and HDFS: Ensure that Hadoop is installed and properly configured on the system where the script is run.
Network Access: The script requires network access to the cloud storage location to fetch data.
Permissions: The user running the script must have sufficient permissions to create directories, move files, and execute Hadoop commands.

Logging
The script logs its progress and any errors to /tmp/sfm_<timestamp>.log. This log file provides detailed information on the execution of the script, which can be used for debugging and auditing purposes.
Future Enhancements
Hive Integration: The script includes commented-out sections for integrating the processed data with Hive tables, allowing for further data analysis and reporting.
Enhanced Validation: Additional validation rules could be implemented to ensure data quality.
Scalability: The script can be extended to handle larger datasets and more complex data processing pipelines.
