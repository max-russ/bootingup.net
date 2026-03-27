---
layout: post
title: "Thoughts on IaC and Layering"
date: 2026-03-27 00:00:00 -0700
categories: Blog update
---

Infrastructure as Code (IaC) has a property that there doesn't seem to be an intuitive answer to: managing holistic state versus managing a subset of state. Terraform seems to have the hardest time with this; when managing state of a deployment, when is it easier to manage just a single application compared to managing all state in one deployment. This becomes exasperated when the underlying infrastructure or platform that the application lives on is also managed by IaC, and perhaps making changes is destructive to the original deployment. Doing all of the management in one deployment does mean that there's less messy dependencies and ordering issues when deprovisioning, but making changes to whole swathes of infrastructure in one unwieldy state ultimately feels like a double edged sword. My bias may come from using Cloudformation, where a single account exists and exports manage relationships between stacks, but that also does not feel like a correct answer for all cases either.

Outside of cloud computing, where all resources are provisioned and managed from a control plane, IaC is an even trickier beast to work with. Managaing configuration with idempotent playbooks, like with Ansible, rather than a state-based resource tracking solution, is easier to wrap ones head around and logically apply settings with precsion. When a computing footprint has been in place for some time, this is definitely a way to go, especially as importing resources to be tracked has its own difficulties, such as making changes to state due to properties and scripting out imports. 

Thinking all of this through, managing different tiers of infrastructure in different state tracking files or even tools makes the most sense. Sprawl of tools is not always a great state with one person, but having a tool that manages hardware provisioning, then platform (like K8s), then finally application deployment (the pods) feels like the right pattern. Each group can can managed with its own repo of code, with seperate repos tracking the common custom resources, and ultimately different release cycles should be maintained for different tiers and layers of infrastructure.
