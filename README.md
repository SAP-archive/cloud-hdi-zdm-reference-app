# Cloud HDI ZDM Reference Application [![Build Status](https://travis-ci.org/SAP/cloud-hdi-zdm-reference-app.svg?branch=master)](https://travis-ci.org/SAP/cloud-hdi-zdm-reference-app)

# Description
Cloud HDI (HANA Deployment Infrastructure) ZDM (Zero-Downtime Maintenance) Reference Application, or `cloud-hdi-zdm-ref-app`, demonstrates how to develop Multi-Target Applications with HDI content, which support zero-downtime updates. It is based on the [Muti-Target Application (MTA)](https://www.sap.com/documents/2016/06/e2f618e4-757c-0010-82c7-eda71af511fa.html) model and follows the [Zero-Downtime Maintenance (ZDM) Adoption Guideline](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/e62731aa735340bfb0c4b7c71b4bf5e7.html) in order to support zero-downtime updates. The update is done by following the [Blue-Green Deployment](https://martinfowler.com/bliki/BlueGreenDeployment.html) process. 

## Blue-green deployment
This repository contains two versions of the reference application: [blue](./cloud-hdi-zdm-ref-app.blue) and [green](./cloud-hdi-zdm-ref-app.green). The green version contains changes on top of the blue version. The blue and green versions of the reference application are [Muti-Target Applications (MTAs)](https://www.sap.com/documents/2016/06/e2f618e4-757c-0010-82c7-eda71af511fa.html). Zero-downtime update is demonstrated by updating the blue version of the application to the green version, following the [blue-green deployment process of MTAs](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/764308c52e68488dac848bae93e9137b.html). The blue-green deployment of MTAs is supported on [SAP Cloud Platform Cloud Foundry (SAP CP CF)](https://cloudplatform.sap.com/enterprise-paas/cloudfoundry.html) and [SAP XS Advanced (XSA)](https://help.sap.com/viewer/4505d0bdaf4948449b7f7379d24d0f0d/2.0.00/en-US/df19a03dc07e4ba19db4e0006c1da429.html) environments. Both environments are following the [Cloud Foundry concepts](https://docs.cloudfoundry.org/concepts/overview.html), so the user and development experience is identical. **Note, however, that zero-downtime maintanance of HDI content is only supported on XSA.**

The blue and green versions of the reference application are build independently by the [MTA Archive Builder](https://help.sap.com/viewer/58746c584026430a890170ac4d87d03b/Cloud/en-US/ba7dd5a47b7a4858a652d15f9673c28d.html). The result from the build of each version of the application is MTA archive (.mtar) file. The build results are located at:
1. [cloud-hdi-zdm-ref-app.blue/mta-jee](./cloud-hdi-zdm-ref-app.blue/mta-jee). Blue version of MTA archive (.mtar) file.
2. [cloud-hdi-zdm-ref-app.green/mta-jee](./cloud-hdi-zdm-ref-app.green/mta-jee). Green version of MTA archive (.mtar) file. 

## Components of the reference application 
Each version of the reference application contains database (db) module and backend module.

### Database module
The database module contains [database artifacts](https://help.sap.com/viewer/825270ffffe74d9f988a0f0066ad59f0/CF/en-US/0f6704d293494e1391ce2ebb1ab59e89.html), which are modeled in specific ZDM manner. The SAP HANA Deployment Infrastructure (HDI) provides a service that enables you to deploy database artifacts to so-called [containers](https://help.sap.com/viewer/4505d0bdaf4948449b7f7379d24d0f0d/2.0.01/en-US/23f1f40731504e7eb7e4ec4b65cbfa71.html). After deployment the artifacts are materialized to database objects, which represent the persistence model of the application. The source code of the blue and green versions of the database modules is located at:
1. [cloud-hdi-zdm-ref-app.db.blue](./cloud-hdi-zdm-ref-app.blue/cloud-hdi-zdm-ref-app.db.blue). Contains blue version of database module. 
2. [cloud-hdi-zdm-ref-app.db.green](./cloud-hdi-zdm-ref-app.green/cloud-hdi-zdm-ref-app.db.green). Contains green version of database module. It contains compatible and incompatible database changes on top of the blue version.

### Backend module
The backend module is a Java web application, which consumes the database objects and provides REST API. The source code of the blue and green versions of the backend modules is located at:
1. [cloud-hdi-zdm-ref-app.backend.jee.blue](./cloud-hdi-zdm-ref-app.blue/cloud-hdi-zdm-ref-app.backend.jee.blue). Contains blue version of backend module.
2. [cloud-hdi-zdm-ref-app.backend.jee.green](./cloud-hdi-zdm-ref-app.green/cloud-hdi-zdm-ref-app.backend.jee.green). Contains green version of backend module.

# Requirements

1. [Git version 2.19.1](https://git-scm.com/downloads) or newer version.
2. [Apache Maven version 3.3.9](https://archive.apache.org/dist/maven/maven-3/3.3.9/binaries/) or newer version.
3. [MTA Archive Builder version 1.1.7](https://tools.hana.ondemand.com/#cloud) or newer version. You can find more information for the MTA Archive Builder [here](https://help.sap.com/viewer/58746c584026430a890170ac4d87d03b/Cloud/en-US/ba7dd5a47b7a4858a652d15f9673c28d.html). Currently it is a java archive (.jar) file. Place it in your home folder (`~/mta-archive-builder.jar`).
4. [XSA Command Line Interface (xs CLI) version 1.0.99](https://help.sap.com/viewer/4505d0bdaf4948449b7f7379d24d0f0d/2.0.03/en-US/addd59069e6f444ca6ccc064d131feec.html) or newer version.
5. Get access to [SAP XS Advanced (XSA) on-premise](https://help.sap.com/viewer/4505d0bdaf4948449b7f7379d24d0f0d/2.0.00/en-US/f472017c780e4db4adcbccc8ba04de05.html) environment, by following the linked guides.
6. Login to the [SAP XSA environment](https://help.sap.com/viewer/400066065a1b46cf91df0ab436404ddc/2.0.02/en-US/c00ed3f74c12479e9f3a8cdeb6e1519a.html). You can find more information for the login command [here](https://help.sap.com/viewer/4505d0bdaf4948449b7f7379d24d0f0d/2.0.03/en-US/c175b5436b7e41d187e9a67d2e30f40b.html). After login an [organization and space](https://docs.cloudfoundry.org/concepts/roles.html) are targeted. The reference application will be deployed in this organization and space.
7. The reference application requires HANA service instances for persistence. For SAP XSA, the SAP HANA database instance is provided out of the box.

# Download and Installation

1. Clone the repository

```
$ git clone https://github.com/SAP/cloud-hdi-zdm-reference-app.git && cd ./cloud-hdi-zdm-reference-app
```

2. Build blue version of cloud-hdi-zdm-ref-app

```
$ cd ./cloud-hdi-zdm-ref-app.blue/mta-jee/
$ java -jar ~/mta-archive-builder.jar --build-target=XSA --mtar=cloud-hdi-zdm-ref-app-blue-<version>.mtar build
$ cd ../../
$ # The result from the build is located at ./cloud-hdi-zdm-ref-app.blue/mta-jee/cloud-hdi-zdm-ref-app-blue-<version>.mtar
```

3. Build green version of cloud-hdi-zdm-ref-app

```
$ cd ./cloud-hdi-zdm-ref-app.green/mta-jee/
$ java -jar ~/mta-archive-builder.jar --build-target=XSA --mtar=cloud-hdi-zdm-ref-app-green-<version>.mtar build
$ cd ../../
$ # The result from the build is located at ./cloud-hdi-zdm-ref-app.green/mta-jee/cloud-hdi-zdm-ref-app-green-<version>.mtar
```

# Running

This section applies the blue-green deployment process step by step and demonstrates how a zero-downtime update of the `cloud-hdi-zdm-ref-app` can be achieved.

1. Deploy blue version 

```
$ xs bg-deploy ./cloud-hdi-zdm-ref-app.blue/mta-jee/cloud-hdi-zdm-ref-app-blue-<version>.mtar -e config.mtaext
```

2. Test blue version on live URL

   After the blue version is deployed it can be tested by checking the web resource exposed by the `cloud-hdi-zdm-ref-app-backend-blue` module. The URL of the web resource is printed in a progress message in the console. Search for message: `Application "cloud-hdi-zdm-ref-app-backend-blue" started and available at <live URL>`. This URL is the so called "live URL", because it is publicly available and customers can use it. Execute the following command to test the blue version on the live URL:

```
$ curl <live URL (cloud-hdi-zdm-ref-app-backend-blue)>/api/v1/cdsPerson/version -k
$ # The response should be `BLUE`
```
```
$ curl <live URL (cloud-hdi-zdm-ref-app-backend-blue)>/api/v1/cdsPerson -k
$ # The response should be `{"cdsPerson":{"id":<ID>,"firstName":"FirstName <ID>","lastName":"LastName <ID>"}}`
```

3. Deploy green version

```
$ xs bg-deploy ./cloud-hdi-zdm-ref-app.green/mta-jee/cloud-hdi-zdm-ref-app-green-<version>.mtar -e config.mtaext
```

4. Test green version on idle URL

   Now both the blue and green versions are deployed and run simultaneously, but expose their web resources on different URLs. The green version can be tested by checking the web resource exposed by the `cloud-hdi-zdm-ref-app-backend-green` module. The URL of the web resource is printed in a progress message in the console. Search for message: `Application "cloud-hdi-zdm-ref-app-backend-green" started and available at <idle URL>`. This URL is the so called "idle URL", because it is not publicly available and customers can not use it. It can be used only internally to test whether the green version of the application is production-ready. If it is production-ready - the blue-green deployment process can be resumed, if not - the blue-green deployment process should be aborted. The resume and abort commands are printed as progress messages in the console. Execute the following commands to test the green version on the idle URL:

```
$ curl <idle URL (cloud-hdi-zdm-ref-app-backend-green)>/api/v1/cdsPerson/version -k
$ # The response should be `GREEN`
```

```
$ curl <idle URL (cloud-hdi-zdm-ref-app-backend-green)>/api/v1/cdsPerson -k
$ # The response should be `{"cdsPerson":{"id":<ID>,"firstName":"FirstName <ID>","lastName":"LastName <ID>"}}`
```

5. Resume the blue-green deployment process

   After the `cloud-hdi-zdm-ref-app-backend-green` application is tested, the blue-green deployment process can be resumed. The exact command is printed as a progress message in the console. Search for `Use "xs bg-deploy -i <process id> -a resume" to resume the process.`. After you found it you should execute it:

```
$ xs bg-deploy -a resume -i <process id>
```

6. Test green version on live URL

   After the blue-green process is resumed the blue version of the `cloud-hdi-zdm-ref-app` is deleted and the live URL is switched from the blue to the green version of the application. Now customers can access the publicly available web resource exposed by the green version of the application. Execute the following commands to test the green version on the live URL:

```
$ curl <live URL (cloud-hdi-zdm-ref-app-backend-green)>/api/v1/cdsPerson/version -k
$ # The response should be `GREEN`
```

```
$ curl <live URL (cloud-hdi-zdm-ref-app-backend-green)>/api/v1/cdsPerson -k
$ # The response should be `{"cdsPerson":{"id":<ID>,"firstName":"Fname <ID>","lastName":"LastName <ID>"}}`
```

```
$ curl <idle URL>/api/v1/cdsPerson -k
$ # The response should be `HTTP: 404`
```

# Known Issues
* Currently there is one backend module implemented in Java. In future will be provided more backend modules implemented in different languages, like Node.js, Python, etc.

# How to obtain support
If you need any support, have any question or have found a bug, please report it in the [GitHub bug tracking system](https://github.com/SAP/cloud-hdi-zdm-reference-app/issues).

# License
This project is licensed under SAP SAMPLE CODE LICENSE AGREEMENT except as noted otherwise in the [LICENSE](./LICENSE) file.

# Further reading
* [ZDM for Multi-Target Applications (MTA) with Database Changes on SAP XSA](https://help.sap.com/viewer/4505d0bdaf4948449b7f7379d24d0f0d/2.0.03/en-US/a7afca80f35c444c8d2e4ab42f5ec06d.html)
* [Muti-Target Application (MTA) model](https://www.sap.com/documents/2016/06/e2f618e4-757c-0010-82c7-eda71af511fa.html)
* [Maintaining HDI Containers](https://help.sap.com/viewer/4505d0bdaf4948449b7f7379d24d0f0d/2.0.01/en-US/23f1f40731504e7eb7e4ec4b65cbfa71.html)
