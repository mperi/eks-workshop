---
title: "Emulate EC2 Spot instance interruption"
chapter: true
weight: 65
draft: false
---

## Emulate EC2 Spot instance interruption

#### Launch instance as part of SpotFleet 

We use this API, as it has feature to issue spot interrupt to EC2.

EC2 instances, as you know can be launched using multiple API's -
 
1.  RunInstances (Launch EC2 instance), <br>
2.  Via Launch Configuration using AutoScalingGroups, and <br>
3.  In case of using EC2 Spot, using Spot Fleet API's.

Depending on your case, one might be a better choice over others.

In this section, we are going to use Spot Fleet API to launch an EC2 Spot instance. We use this specifically because Spot Fleet provides user ability to define TargetCapacity that can be modified. Each time target capacity is adjusted down, Spot Fleet provides an interruption notice to take an EC2 Instance off the list of instances it is maintaining as part of the fleet.
 
 #### Validate instance is part of EKS cluster
 
 #### Increase pods in application to ensure it gets placed on newly launched instance
 (Use application that can be placed only on EC2 Spot instances - easiest way is NodeAffinity?)
  
 #### Interrupt
 
 #### Validate that number of pods are still same. 
 Check the number of pods
 (eks drain would have taken the pod from here and placed elsewhere)
 


