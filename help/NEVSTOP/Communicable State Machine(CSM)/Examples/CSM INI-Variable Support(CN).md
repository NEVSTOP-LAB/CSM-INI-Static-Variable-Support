# CSM INI-Variable Support

## 1. Used as parameters parsed by CSM.vi

### Overview

CSM INI-Variable Support 为 CSM 提供简单易用的配置文件支持功能，使用户能够配置应用程序而无需显式读写配置文件。

本范例用于展示如何在CSM中使用INI-Variable Support可解析参数的功能。

### Introduction

展示如何使用 CSM INI-Variable Support 功能将参数信息固化在 INI 配置文件中。INI-variable 使用 CSM API String 的格式。

变量：${section.variable:defaultValue}
 - section：配置文件中的节名
 - variable：配置文件中的变量名
 - defaultValue：默认值，当配置文件中未定义该变量时使用

特殊情况说明：
 - section 参数为可选。省略时，使用默认配置段 SectionName=LabVIEW。
 - 默认值也为可选，未指定时默认为空字符串（""）。

### Steps

- step1: 生成一个临时的INI文件，使用 CSM - Load Configuration Variables From File.vi 加载该文件。
- step2: 使用一个普通的循环模拟一个CSM模块，使用 CSM - Populate Configuration Variables.vi 将字符串中的变量解析为实际值。运行后，可以比较界面显示的信息是否与注释相同
    - step2.1: 这部分的代码中，注意 section/variable 在配置信息中是不存在的。
    - step2.2: step2.1 部分代码运行的期望结果
    - step2.3: 这部分的代码中，注意 并不只是参数可以使用CSM-INI-Variable Support，任意字段都可以使用。
    - step2.4: step2.3 部分代码运行的期望结果
