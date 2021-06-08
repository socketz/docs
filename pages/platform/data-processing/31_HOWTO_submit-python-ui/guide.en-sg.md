---
title: How to submit a Python job on the Data Processing platform using the OVHcloud manager
slug: submit-python
excerpt: Find out how to create a cluster and run your Apache Spark Python job with Data Processing platform using the OVHcloud manager
section: How to
order: 1
---

**Last updated 04<sup>th</sup> May, 2020**

## Objective
This guide helps you to upload an Apache Spark job in Python to your OVHcloud Object Storage and run your job with the Data Processing page in the Manager.

If you want to submit an Apache Spark job in Java or Scala language, you can read this document: [How to submit a Java/Scala job using Data Processing Manager](../submit-javascala/){.external}

In this guide, we are assuming that you're using the [OVHcloud Manager](https://ca.ovh.com/auth/?action=gotomanager&from=https://www.ovh.com/sg/&ovhSubsidiary=sg){.external} to use the Data Processing platform.

To read an introduction about the Data Processing service you can visit [Data Processing Overview](../overview){.external}.

## Requirements
- Access to [OVHcloud Manager](https://ca.ovh.com/auth/?action=gotomanager&from=https://www.ovh.com/sg/&ovhSubsidiary=sg){.external}
- An OVHcloud account
- A cloud project in your OVHcloud account (see [How to create a cloud project](../../public-cloud/getting_started_with_public_cloud_logging_in_and_creating_a_project){.external} for details.)
- An Openstack user in your cloud project and access to Openstack Horizon dashboard (see [How to create an Openstack user and access to Horizon](../../public-cloud/configure_user_access_to_horizon/){.external} for details.)
- Your application code as Python files
- An environment.yml file in Conda standard.  (Anaconda is a Python distribution that helps you easily manage and share your Python environment and requirements. It contains a lot of Python packages by default and specially the data science related packages. Please visit [this website](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#sharing-an-environment) to learn more about how to create your environment.yml file with Conda format.)

## Instructions

### Step 1: Upload your Python application code and requirements file

Before running your job in the Data Processing platform, you will need to create a container in your Object Storage. Then you will need to upload your application Python files and environment.yml file at the root of this container. You can work with your Object Storage using either the OVHcloud manager or the Openstack Horizon dashboard.

Please see [Creating Storage Containers in Customer Panel](https://docs.ovh.com/gb/en/storage/pcs/create-container/){.external} or [Create an object container in Horizon](../../storage/create_an_object_container/){.external} for more details.


If you don’t currently have an application code and you still would like to try OVHcloud Data Processing, you can download and use the [PI sample from Apache Spark repository](https://github.com/apache/spark/blob/master/examples/src/main/python/pi.py){.external}.

If your application has some package requirements or needs a specific version of Python to run your job, make sure that you mention them in your Conda environment.yml file. You can learn [how to generate environment file here](../generate-environment).

### Step 2: Submit your Apache Spark job
To submit your job with your required parameters follow these steps:

- Login to the OVHcloud manager and select `Public Cloud`{.action}
- Select the relevant project if you have multiple projects in your OVHcloud account
- Select `Data Processing`{.action} from the left panel
- Select `Submit a new job`{.action}

![Data Processing Manager](images/dataprocessingmanager.png){.thumbnail}

- Fill the "**Submit a job**" form that is now displayed and at the end push the `Submit job`{.action} button to submit your Apache Spark job.

Please see [How to fill job submit form in Data Processing Manager](../job-submit-form){.external} for more details.

### Step 3: Check information, status and logs of a job
In the **Data Processing** section of the OVHcloud Manager you can see the list of all the jobs that you have submitted so far. If you click on a job's name, you can see detailed information on it, including its status. Then you can click on the `Logs`{.action} to see the live logs while the job is running.

> [!warning]
> If your jobs are stuck in "Running", you probably forgot to stop the spark context in your code. To stop it, please refer to [pyspark package documentation](https://spark.apache.org/docs/latest/api/python/).

Once the job will be finished, the complete logs will be saved to your Object Storage container. You will be able to download it from your account whenever you would like.

Please see [How to check your job's logs in the Data Processing manager page](../check-logs){.external} for more details.

### Step 4: Check your job's results
After your Spark job is finished, you will be able to check the results from your logs as well as in any connected storage your job was designed to update.

## Go further

To learn more about using Data Processing and how to submit a job and process your data, we invite you to look at [Data Processing documentations page](../).

You can send your questions, suggestions or feedbacks in our community of users on [https://community.ovh.com/en/](https://community.ovh.com/en/){.external} or in our public [Gitter](https://gitter.im/ovh/data-processing){.external}
