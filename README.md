# MonToDate

A generic control-loop framework to maintain monitoring configuration up to date.

## Background & Motivation

The tailoring of a monitoring platform frequently relies on the application of a common approach to a group of entities that can change overtime:

* each Node in the deployment should be provided with a Custom Dashboard that presents a well defined set of key metrics
* each Transaction of a given service must have a health rule around it that captures siginificant deviations from expected values in regards workload and quality of service
* each new Back End should have an associated probe that periodically tries to access it and creates metrics reflecting the results of each attempt over time

These activities are normally performed manually. Unavoidably, as the domains being monitored change, the platform's configuration becomes outdated.

This initiative aims to produce a first prototype of a Control Loop mechanism that allows to maintain monitoring configuration 'up to date' after monitoring requirements that have been submitted to it. The Control Loop:

* assesses the current state of the monitoring domain and the requirements it manages,
* determines if a change justifies a 'refresh' of the implementation of a requirement, and if so
* triggers the generation of the implementation specification and applies it to the target plartform

The following sections describes the key concepts. This will also help to delineate more concisely its use cases.

## Key Concepts

### Monitoring Outcome (MonOut)

A MonOut is the specification of configuration changes that can be applied (as a single operation) to a platform to change its capabilities. MonOuts are normally preserved as files, with one file per MonOut.

In MonToDate, MonOuts are the result of merging a template (that defines the generic structure of the MonOut) with data sources. In this very narrow definition, an Outcome is considered outdated if any, its data sources or its template are different from those that were used when its most recent instantiation was produced.

### Data Sources



### MonOut Application Method (MonOut Apply)

Apply is the method (in the broadest sense) that is used to take a MonOut's representation and uplodad it in its target platform. Apply methods are to be defined as parameterisable plug-ins to MonToDate, with one simple method, a REST API call overt https transport provided as bootstrap.


### Monitoring Requirement (MonReq)

A MonReq expresses a number of configuration outcomes (MonOuts) that address specific requirement. An example, in the AppDynamics world, could represent Outcomes for a given Application producing:

* Business Transaction Detection Rules, after established standards for the application framework
* Transaction Health Rules
* A Custom Transactions Dashboard

The application of MonOuts is sequential, as specified in the requirement. This to allow expressing dependencies among the different requirement components. 

The implementation of a Monitoring Requirement is considered outdated if any of its constituting outcomes is outdated. 


## Supporting Entities

### MonReq Registry

### Template Repository

### DataSource Repository

### MonAut Repository
