CUDA = off

ENABLE_PRECOMPILED_HEADERS = off

doc  = off

openexr = off

//================= These options are for visual studio 

WITH_MSMF = off

ENABLE_PRECOMPILED_HEADERS=OFF

WITH_IPP=OFF

WITH_TBB=OFF

//=================

opencv_extra_modules_path - set to opencv_contrib-3.3.0\modules

//=================

if errors  with - libIlmImf 
set openexr = off

//=================

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

//=================

warnings 
cc1plus.exe: warning: command-line option '-Wmissing-prototypes' is valid for C/ObjC but not for C++
cc1plus.exe: warning: command-line option '-Wstrict-prototypes' is valid for C/ObjC but not for C++

change in OpenCVUtils.cmake
-    "command line option .* is valid for .* but not for C\\+\\+" # GNU
-    "command line option .* is valid for .* but not for C" # GNU
+    "command[- ]line option .* is valid for .* but not for C\\+\\+" # GNU
+    "command[- ]line option .* is valid for .* but not for C" # GNU

//=================
