Simplified Example Voting App

( from https://github.com/dockersamples/example-voting-app )

=========

A simple distributed application running across multiple Docker containers.

Getting started
---------------

Download Docker and Docker-compose

## Linux Containers

The Linux stack uses Python, Node.js, .NET Core (or optionally Java), with Redis for messaging and Postgres for storage.

Run in this directory:
```
docker-compose up
```
The app will be running at [http://localhost:5000](http://localhost:5000), and the results will be at [http://localhost:5001](http://localhost:5001).

Architecture
-----

![Architecture diagram](architecture.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) queue which collects new votes
* A [.NET Core](/worker/src/Worker) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) webapp which shows the results of the voting in real time


Run the app in Kubernetes
The folder k8s-specifications contains the yaml specifications of the Voting App's services.

First create the vote namespace

$ kubectl create namespace vote
Run the following command to create the deployments and services objects:

$ kubectl create -f k8s-specifications/
deployment "db" created
service "db" created
deployment "redis" created
service "redis" created
deployment "result" created
service "result" created
deployment "vote" created
service "vote" created
deployment "worker" created
The vote interface is then available on port 31000 on each host of the cluster, the result one is available on port 31001.


Note
----

The voting application only accepts one vote per client. It does not register votes if a vote has already been submitted from a client.
