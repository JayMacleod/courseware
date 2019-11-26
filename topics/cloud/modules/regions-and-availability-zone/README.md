# Regions and Availability zones

## Overview

*Availability zones* are tightly coupled (related) to *Regions*, hence this module will cover them both alongside each other.

### Availability zones

**Availability zone** (**AZ**) is a physical location within a *region*, each *AZ* is unique. 

One or more data centres make up one *AZ* and has it's own independent power supply, cooling and networking.

*AZ* are independent for a reason, which is to make sure that datacentre failure would be recoverable through copying everything over to another  datacentre within the same *AZ*, this action is called **zone redundancy**.

*Zone redundancy* gives the protection from single point of failure.

Each cloud provider will be guaranteeing a certain up-time of their VMs, in most cases reaching 99.99% or close to it, which will be described in the **S**ervice **L**ayer **A**greement (**SLA**)

### Regions

In order to maximise resiliency, each *region* has multiple *AZ*, the number depends on the cloud provider.

## Tasks

