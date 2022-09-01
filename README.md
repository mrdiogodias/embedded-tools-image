# Docker containers for CI/CD of C/C++ projects

Adapted from [embedded-guidelines](https://git.fh-aachen.de/embedded-guidelines).

This repository contains Dockerfiles that can build common CI/CD utilities for C/C++ projects. The utilities are divided in 2 categories:

- **Compilers**: Compilers, build systems and SDKs for MCUs.
    - [make](https://packages.debian.org/en/sid/make)
    - [gcc-arm-none-eabi](https://packages.debian.org/buster/gcc-arm-none-eabi)
    - [cmake](https://github.com/Kitware/CMake) (3.24.0)
    - [ninja](https://github.com/ninja-build/ninja) (1.11.0)
- **Static analysis**: Utilities to perform Static analysis.
    - [cppcheck](https://github.com/danmar/cppcheck) (2.8)
    - [vera++](https://bitbucket.org/verateam/vera/wiki/Home)
    - [RATS ](https://github.com/andrew-d/rough-auditing-tool-for-security)(2.4)
    - [CodeChecker](https://github.com/Ericsson/codechecker) (6.18.2)
        - [LLVM+Clang](https://github.com/llvm/llvm-project) (12.0.1)
        - [Doxygen](https://www.doxygen.nl/index.html) (1.9.4)
    - [PVS Studio](https://pvs-studio.com/en/pvs-studio/) (7.20)
    - [PMD](https://github.com/pmd/pmd) (6.48.0)
    - [Lizard](https://github.com/terryyin/lizard)
    - [cppclean](https://github.com/myint/cppclean)
    - [metrixplusplus](https://github.com/metrixplusplus/metrixplusplus) (1.1.7) 
    - Env. Variables:
        - `CPPCHECK_HTML` Path where the [cppcheck html script](https://github.com/danmar/cppcheck/blob/main/htmlreport/cppcheck-htmlreport) is installed. Allows to generate html output from cppcheck
        - `METRIXPP_DIR` Metrix-plus-plus directory.
        - `CODECHECKER_ACTIVATE` Path where the python env to activate codechecker is installed.
        - `PMD_BIN` Path where [Pmd](https://github.com/pmd/pmd) is installed.

Note: If no version is given for an utility/SW, it means the latest version is retrieved from the package manager or github.

## Directories and files

    <This repository >/
    ├───Common/                 - Common Docker files to build the images
    │   ├───Compilers.dock      - Dockerfile to install the embedded compilers
    │   ├───Debug.dock          - Dockerfile to install the debuggers
    ├───Embedded/               - Dockerfile to build the Embedded Image
    └───README.md               - this file    

----

By looking at the repository you will notice that there are Dockerfiles with `.dock` extension. The reason is two-fold:

1. Not all of the tools are always required to be installed for a specific environment. For example if your project is desktop based, you wouldnt need the embedded compilers. This allows to only select the tools that you need to use.
2. Better organization. Its easier to organize how the Docker images are built if they are organized based on their purpose.

By default Docker does not have a syntax to include other Dockerfiles. For this reason to include these files you can either manually copy their contents to the Dockerfile you want to build or like in this repository, use the [Dockerfile-plus](https://github.com/edrevo/dockerfile-plus) syntax extension. This extension allows to easily include other dockerfiles to your main Dockerfile (see directory Embedded and ROS for examples).To use this extension you need to add the following env. variables before building with docker:

- `DOCKER_BUILDKIT=1`
- `COMPOSE_DOCKER_CLI_BUILD=1`

## How To Use 

As mentioned in the last section, the dockerfiles for the installation of the tools are located in the `Common` directory. If you need to use any of the tools for a specific project you can manually copy the contents or use the [Dockerfile-plus](https://github.com/edrevo/dockerfile-plus) to include them. 

This repository builds automatically the following images based on the docker files in the `Common` directory.

- Embedded: Image for embedded projects that includes all of the `.dock` files located in the `Common` dir. The image is hosted in this gitlab repository under the following url:

    ```
    registry.git.fh-aachen.de/embeddedtools/embedded-tools-image:core
    ```

  The image can also be built from Docker as follows:

    ```bash
    export DOCKER_BUILDKIT=1
    export COMPOSE_DOCKER_CLI_BUILD=1
    docker build -f Embedded/Dockerfile -t embedded-tools-image:core .
    ```
