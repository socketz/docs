---
title: How to submit a job on the Data Processing platform using the CLI
slug: submit-cli
excerpt: Find out how to run your Apache Spark job with the Data Processing platform using the CLI
section: How to
order: 7
---

**Last updated 14<sup>th</sup> November, 2020**

## Objective
This guide helps you to upload your application code to Object Storage and submit an Apache Spark job using the Data Processing CLI.

To read an introduction about the Data Processing service you can visit [Data Processing Overview](../overview){.external}.

## Requirements
- An OVHcloud account
- An activated cloud project in your OVHcloud account (see [How to create a cloud project](../../public-cloud/getting_started_with_public_cloud_logging_in_and_creating_a_project){.external} and [How to activate the Data Processing service for your cloud project](../activation){.external} for details)
- An Openstack user in your cloud project and access to Openstack Horizon dashboard (see [How to create an Openstack user and access to Horizon](../../public-cloud/configure_user_access_to_horizon/){.external} for details)
- An application code to be run in Apache Spark environment

## Instructions

### Step 1: Download the Data Processing CLI
Download the latest release for your system of the CLI binary available on [GitHub](https://github.com/ovh/data-processing-spark-submit/releases){.external} and save it as `ovh-spark-submit` or run this: 
```shell-session
$ export SYSTEM_ARCHITECTURE=darwin_386 # or darwin_amd64 or linux_386 or linux_amd64 or windows_386 or windows_amd64
$ export DATA_PROCESSING_CLI_VERSION=$(curl -s https://api.github.com/repos/ovh/data-processing-spark-submit/releases/latest | grep "tag_name" | cut -d : -f 2,3 | tr -d \",\ )
$ wget -O ovh-spark-submit  https://github.com/ovh/data-processing-spark-submit/releases/download/$DATA_PROCESSING_CLI_VERSION/ovh-spark-submit_$SYSTEM_ARCHITECTURE
```
If you are on Linux or MacOs, you may have to run this command to make it executable:

```shell-session
$ chmod u+x ovh-spark-submit
```

### Step 2: Set up the configuration.ini file
To be able to submit a job with your CLI, you must set up some configurations that will allow it to authenticate to the OVHcloud API.
To do so, you will need an application key, a secret application key and a consumer key. These can be obtained [here]( https://eu.api.ovh.com/createToken/){.external}.

You need to add the rights `GET/POST/PUT` on the endpoint `/cloud/project/*/dataProcessing/*` .
![Creating API Keys for your script](images/keys.png){.thumbnail}

Once you have got your keys, you have to create a new `configuration.ini` file in the same directory and complete it with your 3 keys.
```
[ovh]
; configuration specific to 'ovh-eu' endpoint as it's the only one available for now
endpoint=ovh-eu
application_key=my_app_key
application_secret=my_application_secret
consumer_key=my_consumer_key
```

### Step 3: Upload your application code to Object Storage
Before running your job in the Data Processing platform, you will need to create a container in OVHcloud Object Storage. 
You can work with your Object Storage using either the OVHcloud Manager or the Openstack Horizon dashboard.

Please see [Creating Storage Containers in Customer Panel](https://docs.ovh.com/gb/en/storage/pcs/create-container/){.external} or [Create an object container in Horizon](../../storage/create_an_object_container/){.external} for more details.

You can also manage your Object storage through command line with the [Openstack Swift API](https://docs.ovh.com/gb/en/public-cloud/getting_started_with_the_swift_api/){.external} 

When it is created, upload your application code and environment file if needed at the root of the container. If you don't have any application code, you can still try the CLI with the examples files provided inside the `testdata` directory of the [GitHub project](https://github.com/ovh/data-processing-spark-submit){.external}.
If you want to submit a python job, do not forget to upload your environment.yml file (see [How to generate environment file for Python jobs](../generate-environment){.external})

>[!primary]
>
> If you only have your application code to upload, you can use the auto-upload feature of the CLI instead of uploading it manually (see **step 4: Submit a job - Optionally use auto-upload**).


### Step 4: Submit a job

Now everything is ready, let's submit a job !

To launch a job with the Data Processing CLI, you have to run the executable file you previously built with your job configurations as parameters.

Here is an example of a command you could run to submit a SparkPi job in java/scala:

```shell-session
$ ./ovh-spark-submit --projectid yourProjectId --class org.apache.spark.examples.SparkPi --driver-cores 1 --driver-memory 4G --executor-cores 1 --executor-memory 4G --num-executors 1 swift://odp/spark-examples.jar 1000
```

In this example, the application code `spark-examples.jar` is stored in the `odp` container on Object Storage, the Spark driver and its executor have 1 core and 4 gibibytes of memory. Once the Spark cluster is deployed the script will run with `1000` as argument.

>[!primary]
>
> Some of the parameters can be set as environment variables, such as ** --projectid**.

> [!warning]
> The different application parameters are stored in plain text. It is advised that you store your credentials in configuration files instead of using arguments. You need to upload the configuration files in the same Object Storage container that you upload the code so they will be downloaded to the data processing cluster together when you submit the job.

The ovh-spark-submit CLI provides a part of the parameters of spark-submit to configure your job.
If you want to know more about these parameters, run:

```shell-session
$ ./ovh-spark-submit -h
```

If you don't know how to set these parameters values, please refer to the page [How to fill the job submit form in the Data Processing page from the OVHcloud Manager](../job-submit-form){.external})

While your job is running, you can watch logs in your terminal or access Spark UI through this URL: 

`https://adc.{region}.dataconvergence.ovh.com/{your-job-id}/jobs/`

Here **region** refers to the region you chose to submit your job.

At any time, you can stop your job by pressing `Ctrl+C`. If you do so, the CLI will ask you to confirm that you want to cancel the job before killing it.

>[!warning]
>
> When streaming logs you are limited to 10 000 character at a time. Meaning you could experience missing logs in streaming mode. All logs will be uploaded to your Object Storage at job end.
>

If you want to check your results after the job is finished, you can download its logs from your Object Storage (see [Checking a job's logs in the Data Processing manager's page](../check-logs)).

#### Optionally use auto-upload
If you want to save time when you often need to change your application code, the auto-upload feature of the CLI allows you to upload your code on Object Storage automatically.
Uploading a file this way into your Object Storage will overwrite previous artefacts with the same name.
To enable it, you need to update the `configuration.ini` file to add the configurations needed for the protocol you want to use.

For now, the only protocol supported is OpenStack Swift. Here is how you should update you `configuration.ini` file in order to upload your code with this protocol:

```
[ovh]
; configuration specific to 'ovh-eu' endpoint as it's the only one available for now
endpoint=ovh-eu
application_key=my_app_key
application_secret=my_application_secret
consumer_key=my_consumer_key

; configuration specific for protocol swift (OVHcloud Object Storage with Keystone v3 authentication)
[swift]
user_name=openstack_user_name
password=openstack_password
auth_url=openstack_auth_url
domain=openstack_auth_url_domain
region=openstack_region
```

And here is an example of a command you could run to run the same job after uploading your local code (`spark-examples.jar`) to your `odp` Object Storage container with the Swift protocol:

```shell-session
$ ./ovh-spark-submit --projectid yourProjectId --upload ./spark-examples.jar --class org.apache.spark.examples.SparkPi --driver-cores 1 --driver-memory 4G --executor-cores 1 --executor-memory 4G --num-executors 1 swift://odp/spark-examples.jar 1000
```

## Go further

To learn more about using Data Processing and how to submit a job and process your data, we invite you to look at [Data Processing documentations page](../).

You can send your questions, suggestions or feedbacks in our community of users on [https://community.ovh.com/en/](https://community.ovh.com/en/){.external} or in our public [Gitter](https://gitter.im/ovh/data-processing){.external}
