# Regions and Availability zones

<!--TOC_START-->
## Contents
- [Overview](#overview)
	- [Availability zones](#availability-zones)
	- [Regions](#regions)
- [Tasks](#tasks)

<!--TOC_END-->
## Overview

*Availability zones* are tightly coupled (related) to *Regions*, hence this module will cover them both alongside each other.

### Availability zones

**Availability zone** (**AZ**) is a physical location within a *region*, each *AZ* is unique. 

One or more data centres make up one *AZ* and has it's own independent power supply, cooling and networking.

*AZ* are independent for a reason, which is to make sure that datacentre failure would be recoverable through copying everything over to another  datacentre within the same *AZ*, this action is called **zone redundancy**.

*Zone redundancy* gives the protection from single point of failure.

Each cloud provider will be guaranteeing a certain up-time of their VMs, in most cases reaching 99.99% or close to it, which will be described in the **S**ervice **L**ayer **A**greement (**SLA**)

An additional benefit of *AZ* is that it has good low latency replication next to high availability, which allows you to make sure the mission-critical applications are running without issues.

One of potential use cases where *AZ* comes into play would be a scenario where you would like to make sure that your database is replicated in other *AZ* for increased resilience. 

This would ensure that if one *AZ* goes down due to some accident, you wouldn't lose your data as it would be available within another *AZ*.

### Regions

A **region** is made up of multiple *AZ* that are interconnected with a low latency network.

In order to maximise resiliency, each *region* has multiple *AZ*, the number depends on the cloud provider, but the typical number is three *AZ* per *region*.

*Regions* give you the benefit of deploying your application in multiple *regions* to increase resilience and reduce latency.

In a scenario where you have half of your user base in one *region*, and another half in a different *region* deploying to both would give the benefits of:
- reduced latency
- increased resilience

Additionally scaling could be set up for each region separately so that it could react to the demands in traffic for each of the regions.

------------------------------------------------------------------------------------------------------------------------
**INSERT IMAGE HERE**
------------------------------------------------------------------------------------------------------------------------

## Tasks

Try answering the following questions (this will involve research):

**Find out what is the typical number of *AZ* within a region for the following cloud providers:**
- Google cloud
- AWS
- Azure

**Who has the most regions currently?**