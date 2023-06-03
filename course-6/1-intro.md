# Understanding Helm

Helm is an open source tool used for packaging and deploying applications on Kubernetes.

It is often referred to as the Kubernetes Package Manager because of its similarities to any other package manager you would find on your favorite OS.

Helm is widely used throughout the Kubernetes community and is a CNCF graduated project.

Given Helm's similarities to traditional package managers, let's begin exploring Helm by first reviewing how a package manager works.

## Understanding Package Managers

Package managers are used to simplify the process of installing, upgrading, reverting, and removing a system's applications. These applications are defined in units, called packages, which contain metadata around target software and its dependencies.

The process behind package managers is simple:

First, the user passes the name of a software package as an argument.

The package manager then performs a lookup against a package repository to see whether that package exists. If it is found, the package manager installs the application defined by the package and its dependencies to the specified locations on the system.

## Installing the Helm Client
Helm provides a single command-line client that is capable of performing all of the main Helm tasks. This client is, appropriately enough, named helm.

The helm client is written in a programming language called Go.

In what follows in the lab, we are going to take a look at the most common workflow for starting out with Helm:

- Install the Helm client.
- Add a chart repository.
- Find a chart to install.
- Install a Helm chart.
- See the list of what is installed.
- Upgrade your installation.
- Delete the installation.
