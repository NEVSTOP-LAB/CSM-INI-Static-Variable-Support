# CSM-INI-Static-Variable-Support

[English](./README.md) | [中文](./README(zh-cn).md)

[![Installs](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/badge.svg?metric=installs)](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/)
[![Stars](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/badge.svg?metric=stars)](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/)
[![Downloads](https://img.shields.io/github/downloads/NEVSTOP-LAB/CSM-INI-Static-Variable-Support/total)](https://github.com/NEVSTOP-LAB/CSM-INI-Static-Variable-Support/releases)

Configuration files are essential components in application development. This library provides simple, user-friendly configuration file support for CSM, enabling users to configure applications without the need to explicitly read from or write to configuration files.

## Installation

Install this library via VIPM. After installation, you'll find the library functions in the CSM Addon palette.

## Design

The architecture of the CSM INI-Static-Variable-Support library is illustrated below:

![Design](.github/csm-ini-variable-cache-design.svg)

Key features include:

1. **Default Configuration Handling**: Automatically loads default configuration files upon first library function call, eliminating the need for explicit loading by users.
2. **Multi-File Support**: Enables loading of multiple configuration files through dedicated functions.
3. **Memory Caching**: Maintains an in-memory cache copy, with applications retrieving configuration information from this cached data.
4. **INI Format Compatibility**: Both configuration files and memory copies use standard INI format, supporting sections and key-value pairs.
5. **Efficient Caching Mechanism**: Uses a global modification flag to optimize performance by only re-reading the memory copy when configurations are modified.

> [!IMPORTANT]
> **Open Source Statement**: This library contains and utilizes a copy of [LabVIEW-Config](https://github.com/rcpacini/LabVIEW-Config) developed by [@rcpacini](https://github.com/rcpacini).

> [!NOTE]
> **Default Configuration File Location**:
>
> - During development: The first INI configuration file found in the Application Directory. If no configuration file exists, it defaults to `csm-app.ini`.
> - After compilation: An INI configuration file with the same name as the executable will be present in the EXE directory (LabVIEW automatically generates this file upon compilation).

> [!NOTE]
> **Multi-File Configuration Scenarios**:
>
> - When loading multiple files, later files will override identical configuration items from previously loaded files.
> - When saving cached changes to files, modifications will be saved to the most recently loaded configuration file.

> [!WARNING]
> Due to the library's use of a global cache modification flag, frequent configuration changes will diminish the effectiveness of the caching mechanism in read VIs. Consequently, this library is not recommended for scenarios requiring frequent configuration modifications.

## Application Scenarios

### CSM-Resolvable Parameters

Enables `${section.variable:defaultValue}` syntax support in CSM, allowing direct use within text messages sent by CSM.

> [!TIP]
>
> - The section parameter is optional. When omitted, the default configuration section `SectionName=LabVIEW` is used.
> - Default values are also optional, with an empty string ("") serving as the default when no value is specified.

![Example](.github/1.png)

### Prototype-Based Configuration Loading

Load configurations by providing a prototype structure. You can load configurations either from an entire section or a specific key.

![Example](.github/2.png)

### Fixed CSM API Parameters

Provides functionality to fix CSM API parameters with a defined priority hierarchy: CSM API parameters > configuration file parameters > default constant parameters. For instance, this makes it easy to fix serial port initialization parameters in a configuration file.

- When initializing, explicitly sent parameters take precedence.
- If no parameters are sent, configuration file parameters are used.
- In the absence of configuration file parameters, default constant parameters are applied.
- Parameters can be partially specified, with missing values automatically filled using the next priority level.

![Example](.github/3.png)

### Multi-File Configuration System

Implement a distributed configuration file system using the multi-file configuration capabilities.

![Example](.github/4.png)

### Referencing Configuration Files with [__include]

The `[__include]` section enables referencing other configuration files, facilitating modular and reusable configuration setups. Included files are preloaded, following the same behavior as multi-file scenarios.

> [!TIP]
>
> - Exercise caution to avoid circular dependencies, which can cause infinite loops. The library maintains a record of loaded configuration files and will skip reloading files that have already been loaded.

![image](.github/6.png)

### Nested Variables

CSM INI-Static-Variable-Support includes support for nested variable parsing, allowing references to other keys within a key for more flexible configuration. The format follows `${section.variable:defaultValue}`.

1. **Read API Behavior**: `CSM INI Read String.vi` retrieves the original configuration value without parsing nested variables. All other read APIs automatically resolve nested variables.
2. **Write API Behavior**: All write APIs overwrite configuration values directly. In general, keys containing nested references should not be directly modified through write operations.

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

Using the configuration above:

**Scenario 1**: Reading `${file.path}` returns an actual file path dynamically assembled from other configuration items, enabling flexible path definitions.

**Scenario 2**: The `[case1]` and `[case2]` sections define two distinct sets of related configuration information. By modifying `${RT.select}`, you can easily switch between these configurations when accessing `${RT.addr}`.

For more detailed usage examples, refer to the sample project.

![image](.github/7.png)