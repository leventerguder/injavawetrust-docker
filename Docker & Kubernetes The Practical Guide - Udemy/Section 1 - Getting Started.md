# What is Docker ? And Why ?

Docker is a container technology : A tool for creating and managing containers.

Container : A standardized unit of software.
A package of code and dependencies to run that code (e.g nodejs code + node js runtime)

The same container always yields the exact same application and execution behavior. No matter where or by whom it might
be executed.

Support for Containers is built into modern operating systems.

Docker simplifies the creation and management of such containers.

# Why Containers ?

Why would we want independent, standardized "application packages" ?

**Different Development & Production Environments**

We want to build and test in exactly the same environment as we later run our app in.

**Different Development Environments Within a Team/Company**

Every team member should have the exactly same environment when working on the same project.

**Clashing Tools/Versions Between Different Projects**

When switching between projects, tools used in project A should not clash with tools used in project B.

## The Problems

Environment : The runtimes, languages, frameworks you need for development

Development Environment <---> Production Environment , often not the same

Development Environment for Employee A <---> Development Environment for Employee B , often not the same

Tools&Libraries required for Project A <---> Tools&Libraries required for Project B , ofen not the same

## We want reliability & Reproducible Environment

- We want to have the exact same environment for development and production. This ensures that it works exactly as
  tested
- It should be easy to share a common development environment/setup with new employees and colleagues.
- We don't want to uninstall and re-install local dependencies and runtimes all the time.