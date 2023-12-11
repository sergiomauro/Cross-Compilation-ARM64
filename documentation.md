# Cross-Compiling C++ Code for ARM64

This guide explains the process of cross-compiling C++ code for ARM64, generating shared objects (.so), and compiling a main program that includes the cross-compiled shared object.

## Prerequisites

Before you begin, ensure you have the following prerequisites:

- Cross-compilation toolchain for ARM64 installed.
- C++ source code files to cross-compile.
- Main C++ source code file that includes the generated shared object.
- Basic knowledge of the cross-compilation process.

## Cross-Compiling C++ Code to .o and .so in x86 Architecture

Firstly, let's recall how to compile the sources into .o files and then into a shared object .so in a native x86 architecture, such as Debian or Ubuntu. Subsequently, we will perform the same process for an ARM64 architecture.

Let's start with the source files "HelloWorld.cpp" and "HelloWorld.h" contained in this repository to perform this process. First, let's execute the command:

``` g++ -c -fPIC -o HelloWorld.o HelloWorld.cpp ```

At this point, starting from the object file .o, it is possible to generate the shared library .so by executing the following command:

``` g++ -shared -o libhelloworld.so HelloWorld.o ```

Finally, assuming that the main.cpp file is located in the same directory as the libhelloworld.so file, we can compile the main.cpp program while integrating the library itself using the following command:

``` g++ -o output_program main.cpp -L. -lhelloworld ```

## Cross-Compiling C++ Code to .o and .so in ARM64 Architecture

After having seen an overview of how we compile a shared object for x86 architectures, we can follow the same procedure to cross-compile the libhelloworld library for the ARM64 target, using the cross-compiler present in the ARM64 toolchain. 

Let's proceed with cross-compiling the helloworld.cpp. 
Let's assume we want to work on a Cortex-A53 processor and utilize the cross-compiler for the Freescale distribution, namely aarch64-fslc-linux-g++
We can cross-compile the file HelloWorld.cpp by executing the following command:

``` [/PATH/FOR/COMPILER/ARM64]/aarch64-fslc-linux-g++ -mcpu=cortex-a53 -march=armv8-a+crc+crypto -fstack-protector-strong  -O2 -D_FORTIFY_SOURCE=2 -Wformat -Wformat-security -Werror=format-security --sysroot=[/PATH/TO/sysroots]/cortexa53-crypto-fslc-linux -c -pipe  -O2 -pipe -g -feliminate-unused-debug-types  --sysroot=[/PATH/TO/sysroots]/cortexa53-crypto-fslc-linux -g -std=gnu++1z -Wall -Wextra -D_REENTRANT -fPIC -I[/PATH/TO/sysroots]/x86_64-fslcsdk-linux/usr/lib/mkspecs/linux-oe-g++ -o HelloWorld.o HelloWorld.cpp ```

The entries highlighted in square brackets ('[]') can be adjusted with the reference paths for your specific cross-compiler and the ARM64 environment's sysroot.

Now, let's proceed by compiling the .o file (or multiple .o files) into the shared object (.so) using the following command:

``` [/PATH/FOR/COMPILER/ARM64]/aarch64-fslc-linux-g++ -shared -mcpu=cortex-a53 -march=armv8-a+crc+crypto --sysroot=[/PATH/TO/sysroots]/cortexa53-crypto-fslc-linux -o libhelloworld.so HelloWorld.o ```

In this case as well, the entries highlighted in square brackets ('[]') can be adjusted with the reference paths for your specific cross-compiler and the ARM64 environment's sysroot.

Now, finally, you can compile your main.cpp file for the ARM64 architecture, including the cross-compiled ARM64 library, by executing the following command:

``` [/PATH/FOR/COMPILER/ARM64]/aarch64-fslc-linux-g++ main.cpp -mcpu=cortex-a53 -march=armv8-a+crc+crypto -fstack-protector-strong  -O2 -D_FORTIFY_SOURCE=2 -Wformat -Wformat-security -Werror=format-security --sysroot=[/PATH/TO/sysroots]/cortexa53-crypto-fslc-linux -Wl,-O1 -Wl,--hash-style=gnu -Wl,--as-needed -Wl,-z,relro,-z,now --sysroot=[/PATH/TO/sysroots]/cortexa53-crypto-fslc-linux -L. -lhelloworld -o output_program_arm64 ```

## Test the Cross-Compiled shared object for ARM64

To test the work achieved in the previous steps, you can move the file 'libhelloworld.so' to the '/usr/lib' folder in your file system. Subsequently, transfer the executable 'output_program_arm64' to your file system.

Grant execution permissions to both the shared object and the executable using the command 'chmod u+x filename'. Finally, execute the file on your ARM64 system



// TO DO
