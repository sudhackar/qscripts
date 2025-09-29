# QScripts Test Add-ons

QScripts streamlines IDA Pro add-on development by automatically reloading plugins, loaders, and processors when their compiled binaries change—no need to restart IDA Pro. This greatly accelerates the development cycle and boosts productivity.

This directory provides example native add-ons (plugins, loaders, processors) show casing QScripts' hot-reload workflow for rapid iteration and testing.


## Available Templates

### [plugin_template](./plugin_template)
Basic plugin template for general IDA Pro plugin development.

### [plugin_triton](./plugin_triton)
Advanced plugin integrating the [Triton](https://github.com/JonathanSalwan/Triton) dynamic binary analysis framework.

### [loader_template](./loader_template)
Template for developing custom file format loaders (with mock `accept_file` and `load_file` implementations).

## Building Add-ons

### Standard Build Process

1. **Navigate to the add-on directory:**
   ```bash
   cd test_addons/<addon_name>
   ```

2. **Configure with CMake:**
   ```bash
   cmake -B build -A x64
   ```

3. **Build the add-on:**
   ```bash
   cmake --build build --config RelWithDebInfo
   ```

The compiled binary will be automatically installed to:
- Plugins: `%IDASDK%/bin/plugins/`
- Loaders: `%IDASDK%/bin/loaders/`
- Processors: `%IDASDK%/bin/procs/`

## Hot-Reload Development Workflow

### 1. Create a Python Script
```python
import time
import idaapi

# Give the linker time to finish
time.sleep(1)

# Load your plugin (adjust name and arguments as needed)
idaapi.load_and_run_plugin('your_addon_name', 0)
```

### 2. Configure Dependencies
Create a `.deps.qscripts` file to specify what triggers reloading:

```
# Monitor the compiled plugin DLL
/triggerfile /keep $env:IDASDK$/bin/plugins/your_addon$ext$
```

### 3. Activate Monitoring
1. Open QScripts chooser
2. Navigate to your Python script
3. Press `Enter` to activate monitoring
4. The script name appears in **bold** when active

### 4. Development Cycle
1. Modify your C++ code
2. Rebuild the add-on
3. QScripts automatically detects changes and reloads
4. No IDA restart required!
