# CSM-INI-Static-Variable-Support

[![Image](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/badge.svg?metric=installs)](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/) 
[![Image](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/badge.svg?metric=stars)](https://www.vipm.io/package/nevstop_lib_csm_ini_static_variable_support/)
[![GitHub all releases](https://img.shields.io/github/downloads/NEVSTOP-LAB/CSM-INI-Static-Variable-Support/total)](https://github.com/NEVSTOP-LAB/CSM-INI-Static-Variable-Support/releases)

The library is now offering `${variable}` support for CSM, which is loaded from the first caught ini file within the application folder or the topmost VI's parent directory. You can either specify a section name or use the pattern of `${section.variable:defaultValue}` if you leave the section name blank.

![image](.github/CSM-INI-Static-Variable-Support.png)

**Open Source Declaration**
 - A copy of [LabVIEW-Config](https://github.com/rcpacini/LabVIEW-Config) by [@rcpacini](https://github.com/rcpacini) is included and used. 
