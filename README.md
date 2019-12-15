Spinal Base Project
============
This repository is a base Gradle project added to help people that prefer Gradle over SBT getting started.

Just one important note, you need a java JDK >= 8

## Quick start

If you already have all the necessary tools installed, clone this repository and run Gradle:

```sh
git clone https://github.com/SpinalHDL/SpinalTemplateSbt.git
cd SpinalTemplateSbt

# If you want to generate the Verilog of your design
./gradlew runVerilog

# If you want to generate the VHDL of your design
./gradlew runVhdl

# If you want to run the scala written testbench
./gradlew runSimulation
```

You'll see the synthesized HDL code in the project's root directory. The simulation output will be written to `./simWorkspace`.

The top level spinal code is defined in `./src/main/scala/mylib`.

## Basics, without any IDE

You don't really need to install Gradle. The executable in the project, `./gradlew` (or `./gradlew.bat`), will download and run the correct Gradle version for you.

If you want to run the scala written testbench, you have to be on linux and have Verilator installed (a recent version).

On Debian:

```sh
sudo apt-get install git make autoconf g++ flex bison -y  # First time prerequisites
git clone http://git.veripool.org/git/verilator   # Only first time
unsetenv VERILATOR_ROOT  # For csh; ignore error if on bash
unset VERILATOR_ROOT  # For bash
cd verilator
git pull        # Make sure we're up-to-date
git checkout verilator_3_916
autoconf        # Create ./configure script
./configure
make -j$(nproc)
sudo make install
cd ..
echo "DONE"
```

On Arch Linux:

```sh
sudo pacman -S verilator
```

## Work in IntelliJ IDEA

This project already contains the Gradle idea plugin. With the `idea` task (`./gradlew idea`), it will generate all files so that you can import this as project in in IntelliJ (`File` – `Open Project`, see [the documentation](https://docs.gradle.org/current/userguide/idea_plugin.html) for more details.

## Work with Eclipse

First, you need to add the [scala plugin](https://scala-ide.org/) to Eclipse (click on the link or simply search the Marketplace). 

This project already contains the Gradle eclipse plugin. With the `eclipse` task (`./gradlew eclipse`), it will generate all files so that you can import this as an existing project in in Eclipse (`File` – `Import…` … `Existing Projects into Workspace`, see [the documentation](https://docs.gradle.org/current/userguide/eclipse_plugin.html) for more details.

In Eclipse, there is also the possibility to install [Buildship, the Gradle plugin for Eclipse](https://projects.eclipse.org/projects/tools.buildship) (not to confuse with the Eclipse plugin for Gradle as above), that allows a more straightforward Gradle-Eclipse integration.

## A word about main methods

In SBT, you run the task `runMain MyClassName` to run the main method you want. One of the few downsides of Gradle is that it is built around the concept of having one main class per project. The solution to this is to define custom tasks, one for each main method to run, in the `build.gradle`:

```Groovy
task myRunTaskName(type: JavaExec) {
    classpath = sourceSets.main.runtimeClasspath
    main = "mypackage.MyMainClassName"
}
```

Then, type `./gradlew myRunTaskName` to run it.
