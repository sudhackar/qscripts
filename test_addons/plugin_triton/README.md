# Triton Plugin for IDA Pro with QScripts

A native IDA Pro plugin that integrates the [Triton](https://github.com/JonathanSalwan/Triton) dynamic binary analysis framework, demonstrating QScripts' hot-reload capabilities for rapid plugin development.

## Quick Start

### Installing Triton on Windows

Let's clone to `C:\tools` for this example:

1. **Install vcpkg** (if not already installed):
   ```batch
   cd C:\tools
   git clone https://github.com/Microsoft/vcpkg.git
   cd vcpkg
   bootstrap-vcpkg.bat
   ```

2. **Clone Triton**:
   ```batch
   cd C:\tools
   git clone https://github.com/JonathanSalwan/Triton.git
   cd Triton
   ```

3. **Build with static runtime** (required for IDA plugins):
   ```batch
   cmake -B build-static -A x64 ^
         -DCMAKE_TOOLCHAIN_FILE="c:\tools\vcpkg\scripts\buildsystems\vcpkg.cmake" ^
         -DVCPKG_TARGET_TRIPLET=x64-windows-static ^
         -DBUILD_SHARED_LIBS=OFF ^
         -DPYTHON_BINDINGS=OFF ^
         -DCMAKE_MSVC_RUNTIME_LIBRARY=MultiThreaded

   cmake --build build-static --config Release
   ```

### Building the Plugin

1. **Set up environment variables** (required):
   ```batch
   set TRITON_INCLUDES=C:\tools\Triton\src\libtriton\includes;C:\tools\Triton\build-static\src\libtriton\includes
   set TRITON_LIB=C:\tools\Triton\build-static\src\libtriton\Release\triton.lib;C:\tools\Triton\build-static\vcpkg_installed\x64-windows-static\lib\libz3.lib;C:\tools\Triton\build-static\vcpkg_installed\x64-windows-static\lib\capstone.lib
   ```

2. **Build the plugin**:
   ```batch
   cmake -B build -A x64
   cmake --build build --config RelWithDebInfo
   ```

3. **Plugin is automatically installed** to `%IDASDK%\bin\plugins\qscripts_native_triton.dll`

4. Activate the `qscripts_native_triton.py` to load the plugin in IDA and start monitoring.
