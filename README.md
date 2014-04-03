<img align="right" src="https://raw.github.com/wiki/ClaireDuSoleil/ZebraCrossing/zxing-logo.png"/>

# ZebraCrossing C++ Port

This is a manual port of ZXing to C++. It has been tested on Linux, Mac OS X and Windows.

The original version of this port can be found [here](https://github.com/zxing/zxing/tree/00f634024ceeee591f54e6984ea7dd666fab22ae).  This new port has been optimized to compile using Visual Studio 2013 and also has been tested on Ubuntu and OS X Mavericks.  Large parts of the original branch have been removed and restructured to make downloading and navigating the source tree faster.  The ZXing library is actively maintained on the master branch: [master](https://github.com/zxing/zxing/tree/master).  Please visit the master branch for the very latest information.


# Building using CMake

Please install [cmake](http://www.cmake.org/cmake/resources/software.html) and make sure it's in your path.  I have used it to build the zxing library on Windows, Ubuntu, and OS X 10.9.2.

This is how cmake works with Visual Studio 2013 to build Win32 and x64 release and debug.  The CMakeLists.txt file is used by cmake to create zxing VS2013 project files that statically link with the MS C runtime.  If you want something different, please edit the CMakeList.txt file.

    cd ZebraCrossing
    mkdir build32
    cd build32
    cmake -G "Visual Studio 12" ..  
    set VisualStudioVersion=12.0
    call "C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin\vcvars32.bat"
    msbuild.exe  libzxing.vcxproj /p:Configuration="Debug" /p:Platform="Win32"
    msbuild.exe  libzxing.vcxproj /p:Configuration="Release" /p:Platform="Win32"
    
    cd ..
    mkdir build64
    cd build64
    cmake -G "Visual Studio 12 Win64" ..
    call "C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\vcvarsall.bat" x64
    msbuild.exe  libzxing.vcxproj /p:Configuration="Release" /p:Platform="x64"
    msbuild.exe  libzxing.vcxproj /p:Configuration="Debug" /p:Platform="x64"


Adjust the above commands as needed for your environment.  If you want to create a batch file from this, you should consider adding some error checking.  On Linux and Mac it will be similar.  If you just run:

     cmake ..
     make

Then you will get the compiler that cmake feels is best when it creates the makefile.  Be careful on Mac because you may need to compile with the option "-stdlib=libstdc++".  Check the Makefile of the main application to make sure you are using the same version of the C++ standard template library, otherwise you will get runtime aborts.  If you want to run cmake interactively to easily add options, run:

    cmake -i ..
    make

The ".." is used to tell cmake where to find the CMakeLists.txt file that it uses to create makefiles and VS project files.  If you use cmake this way, then if you want to start over, just delete the "build" directory and start over.  



