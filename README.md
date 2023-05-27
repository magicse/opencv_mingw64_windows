Fix some errors during compilation openc under windows with mingw

Set in Cmake
```
BUILD_OPENEXR:BOOL=0
BUILD_DOCS:BOOL=0
CMAKE_NM:FILEPATH=C:/mingw64/bin/nm.exe
CMAKE_C_COMPILER_RANLIB:FILEPATH=C:/mingw64/bin/x86_64-w64-mingw32-gcc-ranlib.exe
CMAKE_CXX_COMPILER_AR:FILEPATH=C:/mingw64/bin/x86_64-w64-mingw32-gcc-ar.exe
CMAKE_C_COMPILER_AR:FILEPATH=C:/mingw64/bin/x86_64-w64-mingw32-gcc-ar.exe
CMAKE_CXX_COMPILER_RANLIB:FILEPATH=C:/mingw64/bin/x86_64-w64-mingw32-gcc-ranlib.exe
WITH_CUDA:BOOL=0
CMAKE_AR:FILEPATH=C:/mingw64/bin/ar.exe
CMAKE_CXX_COMPILER:FILEPATH=C:/mingw64/bin/x86_64-w64-mingw32-c++.exe
CMAKE_DLLTOOL:FILEPATH=C:/mingw64/bin/dlltool.exe
CMAKE_CODEBLOCKS_EXECUTABLE:FILEPATH=CMAKE_CODEBLOCKS_EXECUTABLE-NOTFOUND
CMAKE_C_COMPILER:STRING=C:/mingw64/bin/x86_64-w64-mingw32-gcc.exe
OPENCV_EXTRA_MODULES_PATH:PATH=Z:/AI_SDK/CPP_GFPGAN/OPENCV_GIT/opencv/opencv_contrib-3.3.0/modules
CMAKE_CXX_FLAGS_DEBUG:STRING=-g
ENABLE_PRECOMPILED_HEADERS:BOOL=0
WITH_OPENEXR:BOOL=0
CMAKE_CXX_FLAGS:STRING=-std=c++14 -DWIN32
```
//================= This options are for visual studio 
```
WITH_MSMF = off
ENABLE_PRECOMPILED_HEADERS=OFF
WITH_IPP=OFF
WITH_TBB=OFF
```
//=================
```
opencv_extra_modules_path - set to opencv_contrib-3.3.0\modules
```
//=================
```
if errors  with - libIlmImf 
set openexr = off
```
//=================
```
ERROR:
modules/core/src/persistence_base64.cpp:167:31: error: comparing the result of pointer addition ‘(src + ((sizetype)off))’ and NULL [-Werror=address]
  167 |     if (src == 0 || src + off == 0)


Make changes in file OpenCVCompilerOptions.cmake
cmake: don't force -Werror=...
- improve compatibility with further compiler versions
- warnings are not errors by default

new OpenCVCompilerOptions.cmake
-  add_extra_compiler_option(-Werror=return-type)
-  add_extra_compiler_option(-Werror=non-virtual-dtor)
-  add_extra_compiler_option(-Werror=address)
-  add_extra_compiler_option(-Werror=sequence-point)
  add_extra_compiler_option(-Wformat)
-  add_extra_compiler_option(-Werror=format-security -Wformat)

+  add_extra_compiler_option(-Wreturn-type)
+  add_extra_compiler_option(-Wnon-virtual-dtor)
+  add_extra_compiler_option(-Waddress)
+  add_extra_compiler_option(-Wsequence-point)
  add_extra_compiler_option(-Wformat)
+  add_extra_compiler_option(-Wformat-security -Wformat)
```
//=================
```
warnings 
cc1plus.exe: warning: command-line option '-Wmissing-prototypes' is valid for C/ObjC but not for C++
cc1plus.exe: warning: command-line option '-Wstrict-prototypes' is valid for C/ObjC but not for C++

change in OpenCVUtils.cmake
-    "command line option .* is valid for .* but not for C\\+\\+" # GNU
-    "command line option .* is valid for .* but not for C" # GNU
+    "command[- ]line option .* is valid for .* but not for C\\+\\+" # GNU
+    "command[- ]line option .* is valid for .* but not for C" # GNU
```
//=================
```
No x86_64-w64-mingw32-windres.exe

manually copy windres.exe to x86_64-w64-mingw32-windres.exe everything works for now.
```
//=================
```
x86_64-w64-mingw32-windres: unknown option -- W

Solution:
try this : in cmake uncheck ENABLE_PRECOMPILED_HEADERS
```
//=================
```
Error
opencv-3.3.0\modules\videoio\src\cap_dshow.cpp:2302:10: error: 'sprintf_instead_use_StringCbPrintfA_or_StringCchPrintfA'

Solution:
Make changes in file opencv/sources/modules/videoio/src/cap_dshow.cpp 
before this #include "DShow.h":  
add this #define NO_DSHOW_STRSAFE upper of this #include "DShow.h"
```
//=================
```
error include numeric __init = __binary_op(__init, *__first);

Solution:
set -DWIN32
or set -std=c++14 -DWIN32
```
//=================
```
Error
 int, cv::Mat&)':
\opencv_contrib-3.3.0\modules\stereo\src\descriptor.cpp:229:34: error: ordered comparison of pointer with integer zero ('c
onst int*' and 'int')
  229 |             CV_Assert(image.size > 0);
      |                       ~~~~~~~~~~~^~~

\opencv_contrib-3.3.0\modules\stereo\src\descriptor.cpp:230:33: error: ordered comparison of pointer with integer zero ('c
onst int*' and 'int')
  230 |             CV_Assert(cost.size > 0);
      |                       ~~~~~~~~~~^~~

Solution:
opencv\opencv_contrib-3.3.0\modules\stereo\src\descriptor.cpp
in file descriptor.cpp

09	-            CV_Assert(image.size > 0);
10	-            CV_Assert(cost.size > 0);
11	+            CV_Assert(*image.size > 0);
12	+            CV_Assert(*cost.size > 0);
```
//=================
```
Python compiling errors cv2.cpp:854:34: error: invalid conversion from 'const char*' to 'char*'

Solution
Search the output for errors. For me the solution was to add "const" in front of the "char* str", on the cv2.cpp file.

char* str = PyString_AsString(obj);

to

const char* str = PyString_AsString(obj);
```
//=================
```

PROTOBUF compilig error
I had replace -  ReturnCode { SUCCESS, ERROR }; in file command_line_interface_unittest.cc
with ReturnCode { B_SUCCESS, B_ERROR };
or 
with ReturnCode { B_SUCCESS=0, B_ERROR=1 };
and all compiled seccefuly. 
```
