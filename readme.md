# Instrumenting Runtime Data collector for Springboot application

This document explains about how to instrument the Runtime Data Collector in a Springboot application.

Runtime data collectors are embedded within the application workloads that you want to monitor, and can collect deep performance data about the application. This data would be send to MCM hub. From the MCM hub console, we can monitor the applicaiton performance data.

The detailed instruction on how to instrument available in IBM Knolwedge center at https://www.ibm.com/support/knowledgecenter/SSFC4F_1.3.0/icam/config_j2se_dc_intro.html


## Obtain the hub server config info

Obtain the hub server config info using the below url. As as result you might have downloaded with `ibm-cloud-apm-dc-configpack.tar`.

https://www.ibm.com/support/knowledgecenter/SSFC4F_1.3.0/icam/dc_config_server_info.html


## Downloading the J2SE data collector

Downloading the J2SE data collector using the below url. As as result you might have got  `j2se_datacollector.tgz`.

https://www.ibm.com/support/knowledgecenter/SSFC4F_1.3.0/icam/download_j2se_dc.html

## Instrumenting

### Building Docker Image for SpringBoot application.

In this section we are instrumenting the runtime data collector into springboot applicaiton. 

The IBM knowledge center document url is given below.
https://www.ibm.com/support/knowledgecenter/SSFC4F_1.3.0/icam/config_J2SE_dc_monitor_icp_apps.html

### Copy Springboot app jar

Using maven or gradle, you might have created your spring boot jar file.

1. Rename the jar file into `app.jar`

2. Copy `app.jar`  to `files/main` folder. 

2. Copy `app.jar`  to `files/main` folder. 

### Copy Springboot app jar

Using maven or gradle, you might have created your spring boot jar file.

1. Rename the jar file into `app.jar`

2. Copy `app.jar`  to `files/main` folder. 

### Update silent_config_j2se_dc.txt

1. The silent file is available in `files/main/silent_config_j2se_dc.txt`. 

2. Replace the `MAIN_CLASS` variable in the above with with the appropriate springboot main file.

```
MAIN_CLASS=com.cpro.commissionservice.CommissionServiceApplication
```

### Build Docker Image

1. Goto the folder. The silent file is available in `files/main/silent_config_j2se_dc.txt`. 


cp ../target/wealthfinancialplan-0.0.1-SNAPSHOT.jar app.jar

docker build -f ./Dockerfile -t gandigit/wealthcare-financialplan-icam-132-${IMAGE_SUFFIX} . --no-cache

#docker login -u gandigit

docker push gandigit/wealthcare-financialplan-icam-132-${IMAGE_SUFFIX}


a) Create `temp` folder.

b) Copy the `j2se_datacollector.tgz` file that was downloaded in step 1.2 into the `temp` folder. 

c) Create springboot application jar file.

1. Goto the root folder of the application.

2. Build the app and it creates .jar file

    Run `gradle build` , if you have gradle. 

    or

    Run `mvn clean package`, if you have maven. 



### 3.2. Build docker image

a) Goto the above `temp` folder.

b) Run `docker build ......` command to create docker image 

c) Run `docker push ......` command to push the docker image to the registry.

## 4. Deploying Springboot app in the MCM hub

Here are we are going deploy the app in MCM hub. The below kc page contains the info about it.

https://www.ibm.com/support/knowledgecenter/SSFC4F_1.3.0/icam/config_J2SE_dc_monitor_icp_apps.html

### 4.1. Deploy the app

The sample deployment files with Channel and Subscriptions are available at the location `/files/hub/`

a) Replace the `<<IMAGE_NAME>>`, with the actual image that you created above, in the file `/files/hub/21-deployable-bankweb.yaml`.

b Update the managed cluster details in the file `/files/hub/30-placement.yaml`

c) oc apply to all the files available in the folder `files/hub/`

### 4.2. Access the app

a) In the managed cluster, the namespace `fpro-icam-app-ns`, should have some route created. Using the route you can access the app.

b). Create some workload and you can get to see the golden signals in the hub.

