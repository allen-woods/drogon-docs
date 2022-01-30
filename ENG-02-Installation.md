This section takes Linux as an example to introduce the installation process. Other systems are similar;

## System Requirements

- Use `kernel` **version >= 2.6.9, 64-bit** for Linux;
- Use `gcc` **version >= 5.4.0**;
- Use `cmake` **version >= 3.5.0** as the build tool;
- Use `git` as the version management tool;

## Library Dependencies

| Name                                       | Version     | Required | Description                                                                                                                                                                                                                                                                      |
| :----------------------------------------- | :---------- | :------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `boost`                                    | **>= 1.61** |    No    | **Boost is only required if the C++ compiler does not support C++17** and if the STL does not fully support `std::filesystem`.                                                                                                                                                   |
| `c-ares`                                   | stable      |    No    | Can be used to increase Drogon's efficiency with DNS;                                                                                                                                                                                                                            |
| `gtest`                                    | stable      |    No    | Can be used to enable compilation of unit tests.                                                                                                                                                                                                                                 |
| `jsoncpp`                                  | **>= 1.7**  |   Yes    | A JSON library for C++;                                                                                                                                                                                                                                                          |
| `libbrotli`                                | stable      |    No    | Can be used to add support for brotli compression in Drogon when sending HTTP responses;                                                                                                                                                                                         |
| `libuuid`                                  | stable      |   Yes    | A UUID generation library for C;                                                                                                                                                                                                                                                 |
| `openssl`                                  | stable      |    No    | **Can and MUST be used to enable HTTP/TLS support in Drogon.** Drogon only supports HTTP by default.                                                                                                                                                                             |
| `trantor`                                  | stable      |   Yes    | A non-blocking I/O C++ network library also developed by the author of Drogon, provided as a git repository submodule &mdash; no need to install it separately;                                                                                                                  |
| `zlib`                                     | stable      |   Yes    | Used to support compressed transmission;                                                                                                                                                                                                                                         |
| `mariadb`<br />`postgresql`<br />`sqlite3` | stable      |    No    | Client development libraries that can be used to enable databases that Drogon supports.<br />**NOTE: If your use case requires database access, at least one of these libraries MUST be installed.**<br />Drogon does not implicitly support the use of any database by default. |

## System Preparation Examples

### Ubuntu 18.04

#### Environment

```shell
sudo apt install git
sudo apt install gcc
sudo apt install g++
sudo apt install cmake
```

#### jsoncpp

```shell
sudo apt install libjsoncpp-dev
```

#### uuid

```shell
sudo apt install uuid-dev
```

#### OpenSSL

```shell
sudo apt install openssl
sudo apt install libssl-dev
```

#### zlib

```shell
sudo apt install zlib1g-dev
```

### CentOS 7.5

#### Environment

```shell
yum install git
yum install gcc
yum install gcc-c++
```

The default installed cmake version is too low, use source installation

```shell
git clone https://github.com/Kitware/CMake
cd CMake/
./bootstrap && make && make install
```

Upgrade gcc

```shell
yum install centos-release-scl
yum install devtoolset-8
echo "scl enable devtoolset-8 bash" >> ~/.bash_profile
. ~/.bash_profile
```

#### jsoncpp

```shell
git clone https://github.com/open-source-parsers/jsoncpp
cd jsoncpp/
mkdir build
cd build
cmake ..
make && make install
```

#### uuid

```shell
yum install libuuid-devel
```

#### OpenSSL

```shell
yum install openssl-devel
```

#### zlib

```shell
yum install zlib-devel
```

## Database Environment

The libraries below are not mandatory. You can choose to install one or more databases according to your project's needs.

NOTE: Drogon must detect a prior installation of a supported database in your development environment in order to generate code that supports database access during the installation process.

**If you want to develop a webapp that accesses a supported database, please install the necessary libraries into your development environment FIRST and then install Drogon.**

Otherwise, you will encounter a `NO DATABASE FOUND` error.

#### PostgreSQL

The native C library `libpq` needs to be installed to enable support for PostgreSQL. The installation is as follows:

- `ubuntu 16`: `sudo apt-get install postgresql-server-dev-all`
- `ubuntu 18`: `sudo apt-get install postgresql-all`
- `centOS 7`: `yum install postgresql-devel`
- `MacOS`: `brew install postgresql`

#### MySQL

MySQL's native library does not support asynchronous read and write. Fortunately, MySQL also has a version of MariaDB maintained by the original developer community. This version is compatible with MySQL, and its development library supports asynchronous read and write. Therefore, Drogon uses the MariaDB development library to provide the right MySQL support.

**As a best practice, your operating system should not install both MySQL and MariaDB at the same time.**

MariaDB installation is as follows：

- `ubuntu`: `sudo apt install libmariadbclient-dev`
- `centOS 7`: `yum install mariadb-devel`
- `MacOS`: `brew install mariadb`

#### Sqlite3

- `ubuntu`: `sudo apt-get install libsqlite3-dev`
- `centOS`: `yum install sqlite-devel`
- `MacOS`: `brew install sqlite3`

NOTE: Some of the above commands only install the development library of sqlite3. If you want to install a server also, please use Google search yourself.

## Drogon Installation

Assuming that the above environment and library dependencies are all ready, the installation process is very simple;

Once the above preparations have been completed, and given an arbitrary working directory of `$WORK_PATH`, the installation process is as easy as follows:

### Compile debug version

```shell
cd $WORK_PATH
git clone https://github.com/an-tao/drogon
cd drogon
git submodule update --init
mkdir build
cd build
cmake .. # Debug
make && sudo make install
```

### Compile release version

```shell
cd $WORK_PATH
git clone https://github.com/an-tao/drogon
cd drogon
git submodule update --init
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release .. # Release
make && sudo make install
```

### Default system installation paths

After the installation is complete, the following files will be installed in the system（you can change the installation location with the CMAKE_INSTALL_PREFIX option）:

| Path                         | Description                                                          |
| :--------------------------- | :------------------------------------------------------------------- |
| `/usr/local/include/drogon`  | Location of Drogon's header file.                                    |
| `/usr/local/lib`             | Location of Drogon's libdrogon.a library file.                       |
| `/usr/local/bin`             | Location where Drogon's command-line tool `dragon_ctl` is installed. |
| `/usr/local/include/trantor` | Location of trantor's header file.                                   |
| `/usr/local/lib`             | Location of trantor's libtrantor.a library file.                     |

## Include Drogon source code locally

Of course, you can also include the Drogon source code in your project. Suppose you put the Drogon source code under the third_party of your project directory (don't forget to update the git submodule in the Drogon source code's root directory). Then, you only need to add the following two lines to your project's cmake file:

```cmake
add_subdirectory(third_party/drogon)
target_link_libraries(${PROJECT_NAME} PRIVATE drogon)
```

## Use vcpkg

The easiest way to install drogon on windows is to use vcpkg

```
vcpkg.exe install drogon
```

Or

```
vcpkg.exe install drogon:x64-windows
```

**if you haven't installed vcpkg:**

0. [TL;DR](https://www.youtube.com/watch?v=0ojHvu0Is6A)

1. Assuming that you don't have `cmake.exe`, `make.exe` or `vcpkg.exe`

2. Make sure you already have `git` installed &mdash; for Windows too

3. First, go to where you want to install `vcpkg`.

   - For this example, we'll use `C:/Dev` as our directory.
   - If that directory doesn't exist yet, open your **_powershell_** as administrator:
     - type and enter:
       - `cd c:/`
       - `mkdir Dev;cd Dev;` which means you will first create the **Dev** directory, then change to the **Dev** directory
       - `git clone https://github.com/microsoft/vcpkg`
       - `cd vcpkg`
       - `./bootstrap-vcpkg.bat` this will install `vcpkg.exe`
       - NOTE: to update your vcpkg, you just need to type `git pull`
       - to make sure that the vcpkg directory is always available in your **_powershell_** sessions:
         - add `C:/dev/vpckg` to your windows **_environment variables_**.
       - restart/re-open your **_powershell_**

4. Now check if vcpkg already installed properly, just type `vcpkg` or `vcpkg.exe`

5. To install Drogon framework. Type:

   - 32-Bit: `vcpkg install drogon`
   - 64-Bit: `vcpkg install drogon:x64-windows`
   - wait until all dependencies are installed.
   - NOTE:
     - if any package is uninstalled and you get an error, just install that package. e.g.:
       - zlib : `vcpkg install zlib` or `vcpkg install zlib:x64-windows` for 64-Bit
     - to check what already installed:
       - `vcpkg list`

6. To add **_drogon_ctl_** command and dependencies, you need to add some variables. By following this guide, you just need to add:

   ```
   C:\Dev\vcpkg\installed\x64-windows\tools\drogon
   ```

   ```
   C:\Dev\vcpkg\installed\x64-windows\bin
   ```

   ```
   C:\Dev\vcpkg\installed\x64-windows\lib
   ```

   ```
   C:\Dev\vcpkg\installed\x64-windows\include
   ```

   to your windows **_environment variables_**.

7. reload/re-open your **_powershell_**, then type:

   - `drogon_ctl` or `drogon_ctl.exe`
   - press **enter**
   - if:

   ```
   usage: drogon_ctl [-v | --version] [-h | --help] <command> [<args>]
   commands list:
   create                  create some source files(Use 'drogon_ctl help create' for more information)
   help                    display this message
   press                   Do stress testing(Use 'drogon_ctl help press' for more information)
   version                 display version of this tool
   ```

   showed up, you are good to go.

8. Note:
   - you need to be familiar with building cpp libraries by using:
     - independent `gcc` or `g++`
       <br>or
     - Microsoft Visual Studio compiler

<br>

## Use Docker Image

We also provide a pre-build docker image on the [docker hub](https://hub.docker.com/r/drogonframework/drogon). All dependencies of Drogon and Drogon itself are already installed in the docker environment, where users can build Drogon-based applications directly.

## Use Nix Package

There is a Nix package for Drogon which was released in version 21.11.

You can use the package by adding the following `shell.nix` to your project root:

```
{ pkgs ? import <nixpkgs> {} }:
pkgs.mkShell {
  nativeBuildInputs = with pkgs; [
    cmake
  ];

  buildInputs = with pkgs; [
    drogon
  ];
}
```

Enter the shell by running `nix-shell`. This will install Drogon and enter you into an environment with all its dependencies.

The Nix package has a few options which you can configure according to your needs:

| option          | default value |
| --------------- | ------------- |
| sqliteSupport   | true          |
| postgresSupport | false         |
| redisSupport    | false         |
| mysqlSupport    | false         |

Here is an example of how you can change their values:

```
  buildInputs = with pkgs; [
    (drogon.override {
      sqliteSupport = false;
    })
  ];
```

**if you haven't installed Nix:** You can follow the instructions on the [NixOS website](https://nixos.org/download.html).

# 03 [Quick Start](ENG-03-Quick-Start)
