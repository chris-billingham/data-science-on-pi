# Data Science on the Raspberry Pi

This is going to be a kind of rolling document I'll be using as I'm setting up, initially, a Raspberry Pi for doing Data Science. This will include experimentation with various ML Ops tools as well.

### Initial Setup

-   Raspberry Pi OS Lite

-   hostnames is important

-   Set up all the various fstab things

-   Set up AFP and SMB (config stuff)

### Database

Install PostgreSQL (hack away at this <https://community.rstudio.com/t/setting-up-your-own-shiny-server-rstudio-server-on-a-raspberry-pi-3b/18982>)

### R and R Studio

-   Install Java because its comes up <https://linuxize.com/post/install-java-on-raspberry-pi/>

-   Install R but build from source (most of it courtesy of this <https://community.rstudio.com/t/setting-up-your-own-shiny-server-rstudio-server-on-a-raspberry-pi-3b/18982> )

-   Install R Studio for Pi (most of this <https://github.com/ArturKlauser/raspberrypi-rstudio#installing-rstudio-natively-on-your-raspberry-pi> wget this: <https://github.com/ArturKlauser/raspberrypi-rstudio/releases/download/v1.5/rstudio-server-1.2.5033-1.r2r.buster_armhf.deb> )

There does need to be a bit in here where we can build the latest from source rather than waiting on Artur but tbh I'm waiting to do 1.4 for the Python updates

### Python

-   Insert Python

### Orchestration

-   Airflow

-   Argo

### Docker

-   Insert Docker

### ML Ops

-   MLFlow

-   Kubeflow

-   Metaflow
