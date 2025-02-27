# CSM-INI-Static-Variable-Support

[English](./README.md) | [中文](./README(zh-cn).md)

[![Image](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/badge.svg?metric=installs)](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/)
[![Image](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/badge.svg?metric=stars)](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/)
[![GitHub all releases](https://img.shields.io/github/downloads/NEVSTOP-LAB/CSM-INI-Static-Variable-Support/total)](https://github.com/NEVSTOP-LAB/CSM-INI-Static-Variable-Support/releases)

Configuration files is an essential feature for application development. This library aims to provide simple and easy-to-use configuration file support for CSM, allowing users to configure applications through configuration files without explicitly reading or writing them.

## installation

You can install this library through VIPM, and after installation, you can find the functions of this library in the Addon palette of CSM.

## Design

The design of the CSM INI-Static-Variable-Support library is shown in the following figure:

![image](.github/csm-ini-variable-cache-design.svg)

Its features include:

1. Support for default configuration files, which will be implicitly loaded when the library functions are first called, without the need for users to explicitly load configuration files.
2. Support for multiple configuration files, which can be loaded by the functions in Multi-File Support.
3. Create a cache copy in memory, and the actual configuration information read by the application comes from the cache copy.
4. Both the configuration file and the memory copy are in the ini file format, supporting sections and key-value pairs.
5. A global modification flag is created, which helps cache configuration information at the read VI, and the memory copy is read again only when the configuration is modified, which is efficient.

> [!IMPORTANT]
> **Open Source Statement**: Contains and uses a copy of [LabVIEW-Config](https://github.com/rcpacini/LabVIEW-Config) from [@rcpacini](https://github.com/rcpacini).

> [!NOTE]
> The default location of the configuration file:
>
> - In the development state, the first ini configuration file under the Application Directory, and the default configuration file name is csm-app.ini if there is no configuration file.
> - After compilation, the exe directory has an ini configuration file with the same name as the exe. (LabVIEW will inevitably generate such a configuration file after compilation)

> [!NOTE]
> Scenarios with multiple configuration files
>
> - When loading, the later loaded configuration file will override the same configuration items in the previously loaded configuration file.
> - When saving the cache to a file, the changes will be saved to the later loaded configuration file.

> [!WARNING]
> Please note that because this library uses a global cache modification flag, frequent configuration information will cause the cache mechanism at the read VI to fail, so this library is not suitable for scenarios where configuration information is frequently modified.

## Application Scenarios

### Used as parameters parsed by CSM

Provide `${section.variable:defaultValue}` support for CSM, which can be used directly in text messages sent by CSM.

> [!TIP]
> The section can be omitted. By default, the configuration section is SectionName=LabVIEW.
> The defaultvalue can be omitted, and the default value is "" by default.

![image](.github/1.png)

### Load the corresponding configuration by providing the prototype

By providing the prototype to load the corresponding configuration. You can load from the section or key.

![image](.github/2.png)

### Fixed CSM API parameters

Provides the function of fixing CSM API parameters. In this scenario, the priority of CSM API parameters is: CSM API parameters > configuration file parameters > default Constant parameters. For example, in the example, it is easy to fix the parameters of serial port initialization in the configuration file.

- When initialized, if parameters are sent, the sent parameters will be used.
- If no parameters are sent, the parameters in the configuration file will be used.
- If there are no parameters in the configuration file, the default Constant parameters will be used.
- Parameters can be partially provided, and the missing parameters will use the configuration information of the next priority.

![image](.github/3.png)

### Multi-file configuration system

Through the multi-file configuration system, a distributed configuration file system can be implemented.

![image](.github/4.png)
