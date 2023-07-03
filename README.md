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

### Outcome

An Outcome is the specification of configuration changes that can be applied (as a single operation) to a platform to change its capabilities. Outcomes are  preserved as a single file.

In MonToDate, Outcomes are the result of merging a Template (that defines the generic structure of the Outcome) with Data Sources. In this very narrow definition, an Outcome is considered outdated if any, its Data Sources or its Template are different from those that were used when its most recent instantiation was produced.

Templates and merging operation rely on the Apache Velocity engine and template language. Detailed information can be found in [Apache Velocity's project website](https://velocity.apache.org/).

For this prototyp, an Outcome contain:

* An Specification file (as a JSON document)
* A (relative) path to its working area, where copies of Templates, Data Sources and other temporary assets are stored while the Outcome is being processed
* A (relative) path to the Template and Data Source files that were last merged to produce the most recent instance
* A (relative) path to its last instance area, where the most recent merged outcome can be found
* A (relative) path to its target Template
* A (relative) path to its target Data Sources
* A (relative) path to its Apply method
* One Velocity Template
* Zero to many Data Sources
* An Apply method
* An application state (pending, deployed, outdated)
  

### Template

A Template in this context refers to an Apache Velocity template document (one that relies on VTL embedded into a common structure). The template's structure can potentially follow  any specification (XML, JSON, YAML, or even target-specific ones), as needed for the application of the monitoring configuration change on a target platform. 

An Outcome can only specify one Template. A Requirement (see below) contains multiple Outcomes.


### Data Sources

A Data Source is a text file following a particular specification. This prototype aims to support:

* JSON
* XML
* CSV


  

An Outcome can have zero or many Data Sources

### Capture

A Capture is the method (in the broadest sense) used to produce a Data Source.



###  Application Method (Apply)

Apply is the method (in the broadest sense) that is used to take a MonOut's representation and uplodad it in its target platform. Apply methods are to be defined as parameterisable plug-ins to MonToDate, with one simple method, a REST API call overt https transport provided as bootstrap.

The Apply method relies on a common structure (again, a Velocity Template document) and zero-to-many Data Sources. The result of merging Data Sources with the Apply Template should be a script that can be executed from the command line where MonToDate instance executes (in this regard, executing an Apply method equals to run the script produced from that merge operation).





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
