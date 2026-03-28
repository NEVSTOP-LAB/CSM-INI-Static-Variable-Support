# CSM-INI-Static-Variable-Support

[English](./README.md) | [中文](./README(zh-cn).md)

[![Installs](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/badge.svg?metric=installs)](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/)
[![Stars](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/badge.svg?metric=stars)](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/)
[![Downloads](https://img.shields.io/github/downloads/NEVSTOP-LAB/CSM-INI-Static-Variable-Support/total)](https://github.com/NEVSTOP-LAB/CSM-INI-Static-Variable-Support/releases)

Configuration files are a fundamental part of application development. This library provides simple, easy-to-use configuration file support for CSM, allowing users to configure their applications without explicitly reading from or writing to configuration files.

## Installation

Install this library via VIPM. Once installed, the library functions are available in the CSM Addon palette.

## Design

The architecture of the CSM INI-Static-Variable-Support library is illustrated below:

![Design](.github/csm-ini-variable-cache-design.svg)

Key features include:

1. **Default Configuration Handling**: Automatically loads the default configuration file on the first library call, with no explicit loading required.
2. **Multi-File Support**: Load multiple configuration files using dedicated functions.
3. **Memory Caching**: Maintains an in-memory cache; applications read configuration data from this cache.
4. **INI Format Compatibility**: Both configuration files and the memory cache use standard INI format, supporting sections and key-value pairs.
5. **Efficient Caching Mechanism**: Uses a global modification flag so the cache is only refreshed when a configuration change is detected.

> [!IMPORTANT]
> **Open Source Statement**: This library contains and utilizes a copy of [LabVIEW-Config](https://github.com/rcpacini/LabVIEW-Config) developed by [@rcpacini](https://github.com/rcpacini).

> [!NOTE]
> **Default Configuration File Location**:
>
> - During development: The first INI file found in the Application Directory. If none exists, it defaults to `csm-app.ini`.
> - After compilation: The INI file in the EXE directory that shares the executable's name (LabVIEW generates this file automatically at build time).

> [!NOTE]
> **Multi-File Configuration**:
>
> - When multiple files are loaded, later files override matching keys from earlier files.
> - When saving cached changes, modifications are written to the most recently loaded file.

> [!WARNING]
> This library uses a global cache modification flag. Frequent configuration changes reduce the effectiveness of the read caching mechanism. It is not recommended for scenarios that require frequent configuration updates.

## Application Scenarios

### CSM-Resolvable Parameters

Adds `${section.variable:defaultValue}` syntax support to CSM, enabling direct use in text messages sent by CSM.

> [!TIP]
>
> - The `section` parameter is optional. When omitted, the default section `SectionName=LabVIEW` is used.
> - The default value is also optional; an empty string (`""`) is used when not specified.

![Example](.github/1.png)

### Prototype-Based Configuration Loading

Load configurations by providing a prototype structure — either from an entire section or a specific key.

![Example](.github/2.png)

### Fixed CSM API Parameters

Binds CSM API parameters with a defined priority hierarchy: **CSM API parameters > configuration file parameters > default constant parameters**. For example, use this to define default serial port settings in a configuration file.

- Explicitly passed parameters always take the highest priority.
- If no parameters are passed, configuration file values are used.
- If no configuration file values exist, the default constants are applied.

![Example](.github/3.png)

### Multi-File Configuration System

Use the multi-file loading capability to implement a distributed configuration system.

![Example](.github/4.png)

### Referencing Configuration Files with [__include]

The `[__include]` section lets you reference other configuration files, supporting modular and reusable configurations. Referenced files are preloaded using the same behavior as multi-file loading.

> [!TIP]
>
> - Avoid circular dependencies, which can cause infinite loops. The library tracks loaded files and skips any file that has already been loaded.

![image](.github/6.png)

### Nested Variables

CSM INI-Static-Variable-Support supports nested variable parsing, allowing a key's value to reference other keys for more flexible configuration. The syntax is `${section.variable:defaultValue}`.

1. **Read API behavior**: `CSM INI Read String.vi` returns the raw configuration value without resolving nested variables. All other read APIs resolve nested variables automatically.
2. **Write API behavior**: All write APIs overwrite values directly. Keys that contain nested variable references should generally not be modified via write operations.

```ini
// default configuration
[network]
host = ${protocol}://${ip}:${port}
protocol = http
ip = "192.168.0.1"
port = 8080
url = ${host}/API/v1/Get

[case1]
addr = "${network.host}/API/v1/case1/Get"

[case2]
network.host = 127.0.0.1
addr = "${network.host}/API/v2/case2/Get"

[RT]
select = 1
addr = ${case${select}.addr}

[info]
operator = mary
date = #fill by user
time =  #fill by user
test = board

[file]
root = d:/data
path = ${root}/${info.operator}/${info.date}/${info.test}${info.time}.tdms
```

Using this configuration:

**Scenario 1**: Reading `${file.path}` returns a fully resolved file path assembled dynamically from other configuration keys, enabling flexible path definitions.

**Scenario 2**: `[case1]` and `[case2]` define two sets of related configuration. By changing `${RT.select}`, you can switch between them when reading `${RT.addr}`.

For more detailed examples, refer to the sample project.

![image](.github/7.png)