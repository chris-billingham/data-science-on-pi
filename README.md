# Data Science on the Raspberry Pi

This is going to be a kind of rolling document I'll be using as I'm setting up, initially, a Raspberry Pi for doing Data Science. This will include experimentation with various ML Ops tools as well.

### Initial Setup

-   Raspberry Pi OS Lite

-   hostnames is important

-   Set up all the various fstab things

-   Set up AFP and SMB (config stuff)  

Better prompt at this to .bashrc under if ["$color_prompt"] (courtesy of https://www.howtogeek.com/307701/how-to-customize-and-colorize-your-bash-prompt/)  
`PS1='\[\033[1;36m\][\D{%Y-%m-%d} \A] ${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w \$\[\033[00m\] '`

Sort the dates out in ls (partly stolen from http://xahlee.info/linux/bash_prompt_setup.html)  
`alias ls='ls -AlF --color=auto --time-style=long-iso'`

### Database

Install PostgreSQL (hack away at this <https://community.rstudio.com/t/setting-up-your-own-shiny-server-rstudio-server-on-a-raspberry-pi-3b/18982>)

### R and R Studio

-   Install Java because its comes up <https://linuxize.com/post/install-java-on-raspberry-pi/>

-   Install R but build from source (most of it courtesy of this <https://community.rstudio.com/t/setting-up-your-own-shiny-server-rstudio-server-on-a-raspberry-pi-3b/18982> )

-   Install R Studio for Pi (most of this <https://github.com/ArturKlauser/raspberrypi-rstudio#installing-rstudio-natively-on-your-raspberry-pi> wget this: <https://github.com/ArturKlauser/raspberrypi-rstudio/releases/download/v1.5/rstudio-server-1.2.5033-1.r2r.buster_armhf.deb> )

There does need to be a bit in here where we can build the latest from source rather than waiting on Artur but tbh I'm waiting to do 1.4 for the Python updates

**Installing Tidyverse:**

This is needed for openssl/httr/rvest/xml2

    sudo apt-get install -y libssl-dev libxml2-dev libjq-dev libv8-dev

### Python

-   Insert Python ~~3.9~~ 3.8 because you need to check whether downstream packages support 3.9 you idiot

-   <https://linuxize.com/post/how-to-install-python-3-8-on-debian-10/>

-   Redo the symlink to new python

-   `sudo rm /usr/bin/python3`

-   `ln -s /usr/local/bin/python3.8 /usr/bin/python3`

Create symlink for pip3

-   `sudo ln -s /usr/local/bin/pip3.8 /usr/bin/pip3`

### Orchestration

#### Airflow

-   `pip3 install airflow`

-   change backend to postgres as we installed that before (setup the user with a password though): <https://stackoverflow.com/questions/58380835/implementing-postgres-sql-in-apache-airflow>

-   remove the example dags from the db in `airflow.cfg`  

#### Argo

### Docker

-   Insert Docker

### ML Ops

-   MLFlow

-   Kubeflow

-   Metaflow
