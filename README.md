# godot-modules
Godot Modules for the Stigmee project

## CEF - GDNative

GDNative module for CEF integration into Godot, including a demo project

* Tested successfully with Godot 3.4.1-rc
* Along with the following CEF version : cef_binary_96.0.16+g89c902b+chromium-96.0.4664.55_windows64.tar.bz2

### Environment

My environment was set as described below. Please adapt the below commands to your own environment
```
D:\Stigmee\godot-native                        <= godot installation root (compile godot from here)
D:\Stigmee\godot-native\gdcef                  <= gdnative module root (compile the module from here)
D:\Stigmee\godot-native\gdcef\godot-cpp        <= godot c++ bindings clone (compile the c++ bindings from here)
```

### Module folder (./gdcef)

This folder should be placed under the godot installation folder (where godot has been cloned). if contains various sulfolders and files detailed below

```
📦gdcef
 ┣ 📂demo                       <== GODOT DEMO PROJECT using this module, see next section
 ┃ 📂godot-cpp                  <== clone of the godot cpp bindings repository. see below 
 ┃ 📂include                    <== copy of include files of the CEF (not really needed, could use those in thirdparty instead)
 ┃ 📂src                        <== source files of the module
 ┃ ┣ 📜apphandler.cpp
 ┃ ┣ 📜apphandler.h
 ┃ ┣ 📜browser.cpp
 ┃ ┣ 📜browser.h
 ┃ ┣ 📜gdcef.cpp
 ┃ ┣ 📜gdcef.h
 ┃ ┗ 📜gdlibrary.cpp
 ┃ 📂thirdparty                 <== contains the CEF distribution extracted in a cef_binary subfolder
 ┗ 📜SConstruct                 <== Used by scons to build the libgdcef.dll
```
 
First of all, clone the godot-cpp repository into 'godot-cpp' subfolder, using the appropriate branch (do not clone the master as you would end up with headers for the 4.0 version.
Recursive cloning will also include the appropriate godot-headers used to generate the c++ bindings.

```
git clone --recursive -b 3.4 https://github.com/godotengine/godot-cpp
Cloning into 'godot-cpp'...
remote: Enumerating objects: 4763, done.
remote: Counting objects: 100% (955/955), done.
remote: Compressing objects: 100% (417/417), done.
remote: Total 4763 (delta 562), reused 598 (delta 531), pack-reused 3808
Receiving objects: 100% (4763/4763), 3.56 MiB | 15.24 MiB/s, done.
Resolving deltas: 100% (3083/3083), done.
Submodule 'godot-headers' (https://github.com/godotengine/godot-headers) registered for path 'godot-headers'
Cloning into 'D:/Stigmee/godot-exp-04122021/gdnative_cef/godot-cpp/godot-headers'...
remote: Enumerating objects: 801, done.
remote: Counting objects: 100% (144/144), done.
remote: Compressing objects: 100% (111/111), done.
remote: Total 801 (delta 65), reused 74 (delta 23), pack-reused 657
Receiving objects: 100% (801/801), 1.99 MiB | 11.24 MiB/s, done.
Resolving deltas: 100% (498/498), done.
Submodule path 'godot-headers': checked out 'd1596b939d6c9f5df86655ea617713ef321ad938'
```
```
cd godot-cpp
scons platform=windows target=release -j8
```
(use release_debug in 3.4.1-rc)
(use release in stable versions)

To build the library, use the below command from the module root (only tested on Windows at the moment, might need some changes on Linux to build the corresponding .so) :
 
```
D:\Stigmee\godot-native\gdcef>scons platform=windows target=release -j4
scons: Reading SConscript files ...
scons: done reading SConscript files.
scons: Building targets ...
cl /Fosrc\browser.obj /c src\browser.cpp /TP /std:c++17 /nologo -W3 -GR -O2 -EHsc -MD /DWIN32 /D_WIN32 /D_WINDOWS /D_CRT_SECURE_NO_WARNINGS /DNDEBUG /I. /Igodot-cpp\godot-headers /Igodot-cpp\include /Igodot-cpp\include\core /Igodot-cpp\include\gen /I. /Igodot-cpp\godot-headers /ID:\Stigmee\godot-native\godot-cpp\include /ID:\Stigmee\godot-native\godot-cpp\include\core /ID:\Stigmee\godot-native\godot-cpp\include\gen /Ithirdparty\cef_binary\include /Ithirdparty\cef_binary\include\base /Ithirdparty\cef_binary\include\base\internal /Ithirdparty\cef_binary\include\capi /Ithirdparty\cef_binary\include\capi\test /Ithirdparty\cef_binary\include\capi\views /Ithirdparty\cef_binary\include\internal /Ithirdparty\cef_binary\include\test /Ithirdparty\cef_binary\include\views /Ithirdparty\cef_binary\include\wrapper /Isrc
cl /Fosrc\gdcef.obj /c src\gdcef.cpp /TP /std:c++17 /nologo -W3 -GR -O2 -EHsc -MD /DWIN32 /D_WIN32 /D_WINDOWS /D_CRT_SECURE_NO_WARNINGS /DNDEBUG /I. /Igodot-cpp\godot-headers /Igodot-cpp\include /Igodot-cpp\include\core /Igodot-cpp\include\gen /I. /Igodot-cpp\godot-headers /ID:\Stigmee\godot-native\godot-cpp\include /ID:\Stigmee\godot-native\godot-cpp\include\core /ID:\Stigmee\godot-native\godot-cpp\include\gen /Ithirdparty\cef_binary\include /Ithirdparty\cef_binary\include\base /Ithirdparty\cef_binary\include\base\internal /Ithirdparty\cef_binary\include\capi /Ithirdparty\cef_binary\include\capi\test /Ithirdparty\cef_binary\include\capi\views /Ithirdparty\cef_binary\include\internal /Ithirdparty\cef_binary\include\test /Ithirdparty\cef_binary\include\views /Ithirdparty\cef_binary\include\wrapper /Isrc
browser.cpp
cl /Fosrc\gdlibrary.obj /c src\gdlibrary.cpp /TP /std:c++17 /nologo -W3 -GR -O2 -EHsc -MD /DWIN32 /D_WIN32 /D_WINDOWS /D_CRT_SECURE_NO_WARNINGS /DNDEBUG /I. /Igodot-cpp\godot-headers /Igodot-cpp\include /Igodot-cpp\include\core /Igodot-cpp\include\gen /I. /Igodot-cpp\godot-headers /ID:\Stigmee\godot-native\godot-cpp\include /ID:\Stigmee\godot-native\godot-cpp\include\core /ID:\Stigmee\godot-native\godot-cpp\include\gen /Ithirdparty\cef_binary\include /Ithirdparty\cef_binary\include\base /Ithirdparty\cef_binary\include\base\internal /Ithirdparty\cef_binary\include\capi /Ithirdparty\cef_binary\include\capi\test /Ithirdparty\cef_binary\include\capi\views /Ithirdparty\cef_binary\include\internal /Ithirdparty\cef_binary\include\test /Ithirdparty\cef_binary\include\views /Ithirdparty\cef_binary\include\wrapper /Isrc
gdcef.cpp
gdlibrary.cpp
D:\Stigmee\godot-native\gdcef\godot-cpp\include\core\Godot.hpp(163) : warning C4172: retourne l'adresse d'une variable locale ou  temporaire
link /nologo /dll /out:demo\bin\win64\libgdcef.dll /implib:demo\bin\win64\libgdcef.lib /LIBPATH:godot-cpp\bin /LIBPATH:thirdparty\cef_binary\Release /LIBPATH:thirdparty\cef_binary\libcef_dll_wrapper\Release libgodot-cpp.windows.release.64.lib libcef.lib libcef_dll_wrapper.lib src\apphandler.obj src\browser.obj src\gdcef.obj src\gdlibrary.obj
   Création de la bibliothèque demo\bin\win64\libgdcef.lib et de l'objet demo\bin\win64\libgdcef.exp
scons: done building targets.
```

any warning like those below can be ignored :
```
.\include/base/cef_template_util.h(280): warning C4996: 'std::result_of<Functor&&(Args &&...)>': warning STL4014: std::result_of and std::result_of_t are deprecated in C++17. They are superseded by std::invoke_result and std::invoke_result_t. You can define _SILENCE_CXX17_RESULT_OF_DEPRECATION_WARNING or _SILENCE_ALL_CXX17_DEPRECATION_WARNINGS to acknowledge that you have received this warning.
C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\VC\Tools\MSVC\14.30.30705\include\type_traits(1619): note: voir la déclaration de 'std::result_of'
.\include/base/cef_template_util.h(280): warning C4996: 'std::result_of<Functor&&(Args &&...)>': warning STL4014: std::result_of and std::result_of_t are deprecated in C++17. They are superseded by std::invoke_result and std::invoke_result_t. You can define _SILENCE_CXX17_RESULT_OF_DEPRECATION_WARNING or _SILENCE_ALL_CXX17_DEPRECATION_WARNINGS to acknowledge that you have received this warning.
C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\VC\Tools\MSVC\14.30.30705\include\type_traits(1619): note: voir la déclaration de 'std::result_of'
.\include/base/cef_template_util.h(280): warning C4996: 'std::result_of<Functor&&(Args &&...)>': warning STL4014: std::result_of and std::result_of_t are deprecated in C++17. They are superseded by std::invoke_result and std::invoke_result_t. You can define _SILENCE_CXX17_RESULT_OF_DEPRECATION_WARNING or _SILENCE_ALL_CXX17_DEPRECATION_WARNINGS to acknowledge that you have received this warning.
C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\VC\Tools\MSVC\14.30.30705\include\type_traits(1619): note: voir la déclaration de 'std::result_of'
```

When implementing additional CEF features, it will only be needed to change those sources and recompile the lib. Just close the demo project before doing so, or scons will complain that access to the dll file is refused. 

### CefSubProcess.exe - Sub-Process Executable

Whenever CEF is instanced by something else than the Main of the program, if will use the corresponding handle to spawn its subprocesses. 
This means that if CEF is started from a GUI, it will duplicate it indefinitely. To workaround this behavior in situations where changing the main of a program is 
not acceptable, if's advised to use a subprocess executable as described in : 
[https://bitbucket.org/chromiumembedded/cef/wiki/GeneralUsage#markdown-header-separate-sub-process-executable]

in order to create this executable, use the sources contained in *cefSubProcess.zip* along with the CEF sources
Compilation instructions as described by @zexigh :

Compile CEF libraries by extracting the previously downloaded release in cef96 (or use thirdparty/cef_binary and replace the folder name accordingly in the CMakeFile.txt
```
cd cef96
mkdir build
cd build
cmake ..
cmake --build .
```
Compile the subprocess binary
```
cd ../..
mkdir build
cd build
cmake ..
cmake --build .
```

### Demo Project (./gdcef/demo)

GODOT's demo project using the library that we've just compiled:

```
📦demo
 ┣ 📂bin
 ┃ ┣ 📂win64
 ┃ ┃ ┣ 📂locales
 ┃ ┃ ┃ ┗ 📜en-US.pak
 ┃ ┃ ┣ 📜cefSubProcess.exe      <== See CefSubProcess Section
 ┃ ┃ ┣ 📜cefSubProcess.pdb
 ┃ ┃ ┣ 📜cefSubProcess.zip            
 ┃ ┃ ┣ 📜chrome_100_percent.pak
 ┃ ┃ ┣ 📜chrome_200_percent.pak
 ┃ ┃ ┣ 📜chrome_elf.dll
 ┃ ┃ ┣ 📜d3dcompiler_47.dll
 ┃ ┃ ┣ 📜icudtl.dat
 ┃ ┃ ┣ 📜libcef.dll
 ┃ ┃ ┣ 📜libEGL.dll
 ┃ ┃ ┣ 📜libgdcef.dll           <== Shared library used by GDNative module 
 ┃ ┃ ┣ 📜libgdcef.exp
 ┃ ┃ ┣ 📜libgdcef.lib
 ┃ ┃ ┣ 📜libGLESv2.dll
 ┃ ┃ ┣ 📜resources.pak
 ┃ ┃ ┣ 📜snapshot_blob.bin
 ┃ ┃ ┗ 📜v8_context_snapshot.bin
 ┃ ┣ 📜apphandler.gdns          <== CefApp Test
 ┃ ┣ 📜browserview.gdns         <== BrowserView class for use in GDScript (Browser management)
 ┃ ┣ 📜gdcef.gdnlib             <== GDNative descriptor (pointer to dll and dependancies)
 ┃ ┗ 📜gdcef.gdns               <== GDCef class for use in GDScript (CEF management)
 ┣ 📜default_env.tres
 ┣ 📜export_presets.cfg
 ┣ 📜icon.png
 ┣ 📜icon.png.import
 ┣ 📜main.gd                    <== attached main script
 ┣ 📜main.tscn                  <== Project main scene
 ┗ 📜project.godot              <== DEMO project file
```

