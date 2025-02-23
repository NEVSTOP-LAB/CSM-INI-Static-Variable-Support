# CSM-INI-Static-Variable-Support

[English](./README.md) | [中文](./README(zh-cn).md)

[![Image](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/badge.svg?metric=installs)](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/)
[![Image](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/badge.svg?metric=stars)](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/)
[![GitHub all releases](https://img.shields.io/github/downloads/NEVSTOP-LAB/CSM-INI-Static-Variable-Support/total)](https://github.com/NEVSTOP-LAB/CSM-INI-Static-Variable-Support/releases)

该库现在为 CSM 提供 `${variable}` 支持，该变量从应用程序文件夹或最顶层 VI 的父目录中的第一个捕获的 ini 文件加载。您可以指定一个节名称，或者如果您将节名称留空，则使用 `${section.variable:defaultValue}` 模式。

![image](.github/CSM-INI-Static-Variable-Support.png)

**开源声明**
 - 包含并使用了 [@rcpacini](https://github.com/rcpacini) 的 [LabVIEW-Config](https://github.com/rcpacini/LabVIEW-Config) 的副本。
