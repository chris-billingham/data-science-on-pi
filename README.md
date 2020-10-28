# Data Science on the Raspberry Pi

This is going to be a kind of rolling document I'll be using as I'm setting up, initially, a Raspberry Pi for doing Data Science. This will include experimentation with various ML Ops tools as well.

To give some context, I don't have much Linux/Comp Sci experience outside of what is needed to make Docker do the things you need a (or some) Docker to do. My learning style is do, fail, learn, repeat very loudly and I'm sure I'll be making errors that look painfully obvious to many. Ultimately this document will be translated into something more formal like a series of blog posts w. scripting once I've got my head around everything I'm trying to do. But for now, buckle up!

### Initial Setup

-   Raspberry Pi OS Lite

-   hostnames is important, but why have i got `.local` post-pended now? (something something `avahi`?)

-   Set up all the various fstab things

-   Set up AFP and SMB (config stuff)

Better prompt at this to .bashrc under if ["\$color\_prompt"] (courtesy of <https://www.howtogeek.com/307701/how-to-customize-and-colorize-your-bash-prompt/>)\
`PS1='\[\033[1;36m\][\D{%Y-%m-%d} \A] ${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w \$\[\033[00m\] '`

Sort the dates out in ls (partly stolen from <http://xahlee.info/linux/bash_prompt_setup.html>)\
`alias ls='ls -AlF --color=auto --time-style=long-iso'`

### Database

Install PostgreSQL (hack away at this <https://community.rstudio.com/t/setting-up-your-own-shiny-server-rstudio-server-on-a-raspberry-pi-3b/18982>)

### R and R Studio

-   Install Java because its comes up <https://linuxize.com/post/install-java-on-raspberry-pi/> (Note to self god imagine what getting `RJava` to work is going to be like, my magic fixing spells only work on MacOS)

-   Install R but build from source (most of it courtesy of this <https://community.rstudio.com/t/setting-up-your-own-shiny-server-rstudio-server-on-a-raspberry-pi-3b/18982> )

-   Install R Studio for Pi (most of this <https://github.com/ArturKlauser/raspberrypi-rstudio#installing-rstudio-natively-on-your-raspberry-pi> wget this: <https://github.com/ArturKlauser/raspberrypi-rstudio/releases/download/v1.5/rstudio-server-1.2.5033-1.r2r.buster_armhf.deb> )

There does need to be a bit in here where we can build the latest from source rather than waiting on Artur but tbh I'm waiting to do 1.4 for the Python updates. At some point I'm going to end up doing this myself but need a bit of a run up.

**Installing Tidyverse:**

This is needed for openssl/httr/rvest/xml2

    sudo apt-get install -y libssl-dev libxml2-dev libjq-dev libv8-dev

### Installing Apache Arrow both C++ and R Package

Wow this was quite the journey. So this key to getting it going was using this <https://raspberrypi.stackexchange.com/questions/110958/error-in-pyarrow-while-installing-apache-beam-in-raspberry-pi?newreg=c1e66818f67c4447885d6f5a5aea0038> with the answer from the dude that is Stuart, answered Oct 18 at 22:53.

Things we have learned:

-   Don't think you're smart and install LLVM v11.0.0 over v10.0.1 as it had just come out because then you're **not** following instructions and need to be shot. What ended up happening was that I *appeared* to be running out of memory of some kind (both normal and VM [despite bumping the swap RAM up to 2GB]) which was causing the very end of the build from `make -j4` to keel over at around 97%. Changing to just `make` or using `make -l 1` again wasn't helpful. I also tried changed the linker from `LD` to `gold` with `-DLLVM_USE_LINKER=gold` in CMake, plus adding `-DLLVM_TARGETS_TO_BUILD=ARM` (most of that info from <https://stackoverflow.com/questions/25197570/llvm-clang-compile-error-with-memory-exhausted>) but LLVM 11 just didn't want any. So I went back and followed the instructions (but did leave in gold linker and the targets to build) for 10.0.1 and it installed fine. I had been running the installation in a `tmux` session so I wasn't sure whether that was doing it but hey ho, it worked... by following the instructions.

-   Following the above to install Arrow C++ libraries was 100% fine with no changes other than building v2.0.0 which had just come out but it worked fine. HOWEVER, for some reason whenever I would trying and install the R `arrow` package I'd get `libarrow.so.200: undefined symbol: __atomic_load_8` right at the end. Oh boy did I try a tonne of things, adding folders to `LD_LIBRARY` so that it could see the libatomic, rebuiling arrow and pointing to libatomic in the CMake. Tonnes. Just wouldn't have any of it. In the end, (after having to fix R not loading due to not knowing where `libRblas.so` so had to `export LD_LIBRARY_PATH="/usr/local/lib/R/lib:$LD_LIBRARY_PATH"` but I'm sure I messed that up sodding around with `libatomic`), I bodged it by hacking at the last bit to load R like `LD_PRELOAD=/usr/lib/arm-linux-gnueabihf/libatomic.so.1.2.0 R` then installing the `arrow` package at the CLI. Which of course then went fine and seems to have worked. I don't know if Dimitry's comment at <https://raspberrypi.stackexchange.com/questions/110958/error-in-pyarrow-while-installing-apache-beam-in-raspberry-pi?newreg=c1e66818f67c4447885d6f5a5aea0038#comment203248_117723> would've stopped the pain but I had got minor PTSD from it all and had something that works. Something for another day. (Having recovered a bit <https://stackoverflow.com/questions/31381892/fedora-22-compile-atomic-is-lock-free> seems pertinent).

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

-   if things don't work in Daemon mode sometimes you have to go into the `airflow` folder and deleted the airflow-\*\*\*.pid file and it starts working again.

#### Argo

### Docker

-   Insert Docker

### ML Ops

-   MLFlow - Actually installing this is pretty easy following: <https://mlflow.org/docs/latest/quickstart.html> also steal and convert this (<https://stackoverflow.com/questions/58380835/implementing-postgres-sql-in-apache-airflow>) to set up the server with the postgres backend we're using for airflow (<https://mlflow.org/docs/latest/tracking.html#tracking-server>)\
    `mlflow server \     --backend-store-uri postgresql+psycopg2://{user}:{pass}@modern-life-is-rubbish.local:5432/mlflow \     --default-artifact-root /home/pi/nas-share/mlflow \     --host 0.0.0.0` Use this to get it added to systemd as it doesn't have a headless mode: <https://towardsdatascience.com/setup-mlflow-in-production-d72aecde7fef> or <https://pedro-munoz.tech/how-to-setup-mlflow-in-production/> Ok this has proved more difficult for some reason I cannot work out. I'll `tmux` it for now and come back to that

-   Kubeflow

-   Metaflow
