<img align="right" src="https://raw.github.com/wiki/zxing/zxing/zxing-logo.png"/>

# ZebraCrossing C++ Port

This is a manual port of ZXing to C++. It has been tested on Linux, Mac OS X and Windows.

The original version of this port can be found [here](https://github.com/zxing/zxing/tree/00f634024ceeee591f54e6984ea7dd666fab22ae).  This new port has been optimized to compile using Visual Studio 2013 and also has been tested on Ubuntu and OS X Mavericks.  Large parts of the original branch have been removed and restructured to make downloading and navigating the source tree faster.  The ZXing library is actively maintained on the master branch: [master](https://github.com/zxing/zxing/tree/master).  Please visit the master branch for the very latest information.


# Building using CMake

Please install [cmake](http://www.cmake.org/cmake/resources/software.html) and make sure it's in your path.  I have used it to build the zxing library on Windows, Ubuntu, and OS X 10.9.2.

This is how cmake works with Visual Studio 2013 to build Win32 release and debug:

  1. cd to root directory or distribution
  2. mkdir build
  3. cd build
  4. cmake -G "Visual Studio 12" ..  
  5. set VisualStudioVersion=12.0
  6. call "C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin\vcvars32.bat"
  7. msbuild.exe  libzxing.vcxproj /p:Configuration="Debug" /p:Platform="Win32"
  8. msbuild.exe  libzxing.vcxproj /p:Configuration="Release" /p:Platform="Win32"


Adjust the above commands as needed for your environment.  On Linux and Mac it will be similar.  If you just run:

     cmake ..

Then you will get the compiler that cmake feels is best when it creates the makefile.



