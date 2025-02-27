---
title: Install
layout: default
---
<!--
    Except where otherwise noted, content in this website is Copyright (c)
    2015-2019, RTE (http://www.rte-france.com) and licensed under a
    CC-BY-4.0 (https://creativecommons.org/licenses/by/4.0/)
    license. All rights reserved.
-->

Dyna&omega;o is available on **Linux** and **Windows**. For **MacOS** users we recommend to use [Docker](#docker).
The latest release is [Dyna&omega;o v1.6.0]({{ '/release_note' }}) and could be retrieved using the following links:

|---|---|
| Linux distribution | [Dynawo_Linux_v1.6.0.zip](https://github.com/dynawo/dynawo/releases/download/v1.6.0/Dynawo_Linux_v1.6.0.zip) |
| Windows distribution (VS2019) | [Dynawo_Windows_v1.6.0.zip](https://github.com/dynawo/dynawo/releases/download/v1.6.0/Dynawo_Windows_v1.6.0.zip) |
| Documentation | [DynawoDocumentation.zip](https://github.com/dynawo/dynawo/releases/download/v1.6.0/DynawoDocumentation.zip) |
| Dynawo Modelica library | [Dynawo_Modelica_library_v1.6.0.zip](https://github.com/dynawo/dynawo/releases/download/v1.6.0/Dynawo_Modelica_library_V1.6.0.zip) |
| Detailed release note | [v1.6.0_release_note.txt](https://github.com/dynawo/dynawo/releases/download/v1.6.0/v1.6.0_release_note.txt) |


To get started with Dyna&omega;o you have different possibilities, depending on your background and what you want to do:
- If you are interested in the models available and want to have a quick look to them, please open the Dyna&omega;o Modelica library in OpenModelica for example.
- If you want to launch simulations and examples with Dyna&omega;o and observe the performances, you can use the pre-built distributions and the examples directory.
- If you want to checkout the repository and build it yourself to be able to modify the tool, please follow the build instructions available.

### Dyna&omega;o Linux binaries distribution

Official Linux-based release is available [here](https://github.com/dynawo/dynawo/releases/download/v1.6.0/Dynawo_Linux_v1.6.0.zip).

Dyna&omega;o is tested on **Fedora** and **Ubuntu** based platforms.
However, provided that you can install system packages there should be no problem on others Linux distributions.

Required dependencies are the following:

* Compilers: C and C++ ([gcc](https://www.gnu.org/software/gcc) or [clang](https://clang.llvm.org)), c++11 compatible for C++ standard

* Python2 or Python3

* Utilities: [curl](https://curl.haxx.se) and unzip

* [CMake](https://cmake.org/) (minimum version 3.9.6)

Following commands can be used to install the required dependencies:

Ubuntu:

``` bash
$> apt-get install -y g++ unzip curl python
```

Fedora:

``` bash
$> dnf install -y gcc-c++ unzip curl python
```

Following commands can be used to download and test the latest distribution:

``` bash
$> curl -L $(curl -s -L -X GET https://api.github.com/repos/dynawo/dynawo/releases/latest | grep "Dynawo_Linux" | grep url | cut -d '"' -f 4) -o Dynawo_Linux_latest.zip
$> unzip Dynawo_Linux_latest.zip
$> cd dynawo
$> ./dynawo.sh jobs-with-curves sources/examples/DynaWaltz/IEEE14/IEEE14_GeneratorDisconnections/IEEE14.jobs
$> ./dynawo.sh help
$> ./dynawo.sh jobs --help
```

### Dyna&omega;o Windows binaries distribution

Official Windows-based release is available [here](https://github.com/dynawo/dynawo/releases/download/v1.6.0/Dynawo_Windows_v1.6.0.zip).

Dyna&omega;o is tested on **Windows 10**.

If you plan to use Dyna&omega;o with the default models library there is no additional dependency.

If you plan to compile on the fly your own Modelica models then required dependencies are the following:

* [Visual Studio 2019](https://visualstudio.microsoft.com)

* [CMake](https://cmake.org/) (minimum version 3.9.6)

* [Python2](https://www.python.org/ftp/python/2.7.17/python-2.7.17.amd64.msi) or [Python3](https://www.python.org/ftp/python/3.8.5/python-3.8.5-amd64.exe)

You can do as follows to download and test Dyna&omega;o:

* Download the zip of the distribution and unzip it somewhere

* Open either **Command Prompt** or **x64 Native Tools Command Prompt for VS2019** (to be able to use your own models)

* Use **cd** to browse the directory previously unzipped. A file named **dynawo.cmd** should be there.

* Use following commands to launch a simulation:

``` bash
$> dynawo --jobs-file sources\examples\DynaWaltz\IEEE14\IEEE14_GeneratorDisconnections\IEEE14.jobs
```

### Building Dyna&omega;o from sources on Linux

Dyna&omega;o and its dependencies will need some packages to work. Here is the list of all packages you can install to have no dependency problem in the following steps. This example works for Ubuntu:

``` bash
$> apt-get install -y git gcc g++ gfortran autoconf pkgconf automake make libtool cmake hwloc openjdk-8-jdk libblas-dev liblpsolve55-dev libarchive-dev doxygen doxygen-latex liblapack-dev libexpat1-dev libsqlite3-dev libxerces-c-dev zlib1g-dev gettext patch clang python-pip libncurses5-dev libreadline-dev libdigest-perl-md5-perl unzip gcovr lcov libboost-all-dev qt4-qmake qt4-dev-tools lsb-release libxml2-utils python-lxml python-psutil wget libcurl4-openssl-dev rsync
```
This one works for Fedora:
``` bash
$> dnf install -y git gcc gcc-c++ gcc-gfortran autoconf automake make libtool cmake hwloc java-1.8.0-openjdk-devel blas-devel lapack-devel lpsolve-devel expat-devel glibc-devel sqlite-devel xerces-c-devel libarchive-devel zlib-devel doxygen doxygen-latex qt-devel gettext patch wget python-devel clang llvm-devel ncurses-devel readline-devel unzip perl-Digest-MD5 vim gcovr python-pip python-psutil boost-devel lcov gtest-devel gmock-devel xz rsync python-lxml graphviz libcurl-devel
```

To build Dyna&omega;o you need to clone this repository and launch the following commands in the source code directory:

``` bash
$> git clone https://github.com/dynawo/dynawo.git dynawo
$> cd dynawo
$> echo '#!/bin/bash
export DYNAWO_HOME=$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)

export DYNAWO_SRC_OPENMODELICA=$DYNAWO_HOME/OpenModelica/Source
export DYNAWO_INSTALL_OPENMODELICA=$DYNAWO_HOME/OpenModelica/Install

export DYNAWO_LOCALE=en_GB
export DYNAWO_RESULTS_SHOW=true
export DYNAWO_BROWSER=firefox

export DYNAWO_NB_PROCESSORS_USED=1

export DYNAWO_BUILD_TYPE=Release
export DYNAWO_CXX11_ENABLED=YES

$DYNAWO_HOME/util/envDynawo.sh $@' > myEnvDynawo.sh
$> chmod +x myEnvDynawo.sh
$> ./myEnvDynawo.sh build-user
```

You can have more information about compilation options and customisation [here](compilation_options).

**Warning**: If you're working behind a proxy make sure you have exported the following proxy environment variables
``` bash
$> export http_proxy=http://login:mdp@proxy_address:proxy_port/
$> export https_proxy=https://login:mdp@proxy_address:proxy_port/
$> export no_proxy=localhost,127.0.0.0/8,::1
$> export HTTP_PROXY=$http_proxy;export HTTPS_PROXY=$https_proxy;export NO_PROXY=$no_proxy;
```

Once you have installed and compiled Dyna&omega;o as explained in the previous
part, you can launch a simulation by calling one example from DynaFlow, DynaSwing or DynaWaltz:

``` bash
$> ./myEnvDynawo.sh jobs examples/DynaWaltz/IEEE14/IEEE14_GeneratorDisconnections/IEEE14.jobs
```

``` bash
$> ./myEnvDynawo.sh jobs examples/DynaSwing/IEEE14/IEEE14_Fault/IEEE14.jobs
```

``` bash
$> ./myEnvDynawo.sh jobs examples/DynaFlow/IEEE14/IEEE14_DisconnectLine/IEEE14.jobs
```

This command launches a simple simulation on the IEEE 14-bus network that should work if your installation went well and your compilation finished successfully. It could be checked by looking to the outputs directory and comparing its content with the ones from the reference outputs directory (especially the curves file).

``` bash
$> cd examples/DynaWaltz/IEEE14/IEEE14_GeneratorDisconnections/
$> ls outputs
$> diff outputs/curves/curves.csv reference/outputs/curves/curves.csv
```

All the simulation outputs are stored into the outputs directory.

It is also possible to display directly simulation results - plots - into a simple GUI (created for demonstration purpose) by using the following command:

``` bash
$> ./myEnvDynawo.sh jobs-with-curves examples/DynaWaltz/IEEE14/IEEE14_GeneratorDisconnections/IEEE14.jobs
```

For example, for the generator disconnections simulated with DynaWaltz, the plot for the voltage module in p.u. on bus 1 should look like this:

![image](../assets/images/VoltageModule.png "Voltage module in p.u. on bus 10"){: .center-image}

You can obtain more informations about commands you can use by launching:
``` bash
$> ./myEnvDynawo.sh help
```

**We advise you to deploy our autocompletion** script to help you with the available commands, it will also set an alias in your bashrc or zshrc to be able to call Dyna&omega;o from anywhere. You can launch one of the two following commands:
``` bash
$> ./myEnvDynawo.sh deploy-autocompletion --deploy --shell-type bash
$> ./myEnvDynawo.sh deploy-autocompletion --deploy --shell-type zsh
```

Then you can launch:
``` bash
$> dynawo help
```

### Dyna&omega;o Docker environment
<a name="docker"></a>

We provide on [Docker Hub](https://hub.docker.com/r/dynawo/dynawo) an image of Dyna&omega;o master. You can use it by launching the following command:

``` bash
$> docker run -it dynawo/dynawo
```
You can have more information on how to use Docker to build and try Dyna&omega;o [here](https://github.com/dynawo/dynawo-docker).
