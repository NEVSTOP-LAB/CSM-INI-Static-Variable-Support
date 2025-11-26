# CSM INI-Variable Support

## CSM 可解析参数(1. Used as parameters parsed by CSM.vi)

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

## 提供参考Cluster的配置加载 (2. Load the corresponding configuration by providing the prototype.vi)

### Overview

本范例展示通过提供Cluster prototype 加载配置。您可以从整个节或特定键加载配置。

### Introduction

可以提供一个Cluster prototype 来加载配置。

支持这个功能的函数为：
 - CSM - Read Cluster Elements From Session.vim：Cluster prototype 中的元素名称会被作为配置文件中的变量名。
 - CSM - Read Cluster Elements From Key.vim：需要提供Key 参数，cluster 使用API String 格式存储在给定的section/key 中。

特殊情况说明：
- 如果配置文件中未定义该变量，将使用提供原型的Cluster的元素数据。

### Steps

- step1: 生成一个临时的INI文件，使用 CSM - Load Configuration Variables From File.vi 加载该文件。
- step2: 使用 CSM - Read Cluster Elements From Session.vim 加载配置。
- step3: 使用 CSM - Populate Configuration Variables.vi 将字符串中的变量解析为实际值。

## 固化CSM API参数(3. In CSM API parameters.vi)

### Overview

本范例展示如何使用CSM INI-Variable Support 功能将参数信息固化在 INI 配置文件中。

### Introduction

展示如何使用CSM INI-Variable Support 功能将参数信息固化在 INI 配置文件中。只要依靠 Convert API String to Cluster(Default in Session).vim 或 Convert API String to Cluster(Default in Key).vim 来实现。他们和 CSM API String Support 中同名的函数功能相同，但是添加了读取INI配置文件的功能。

在此情况下，数据的来源可能是：1. 通过消息参数发送过的信息; 2. 当前编写时提供的默认常量参数；3. 配置文件中的参数。他们具有明确的优先级层次：CSM API 参数 > 配置文件参数 > 默认常量参数。需要注意：
- 初始化时，显式发送的参数具有最高优先级。
- 若未发送参数，则使用配置文件中的参数。
- 在没有配置文件参数的情况下，应用默认常量参数。
- 参数可以部分指定，缺失的值会自动使用下一优先级的配置信息填充。

### Steps

- step1: 生成一个临时的INI文件，使用 CSM - Load Configuration Variables From File.vi 加载该文件。
- step2: 使用一个普通的循环模拟一个CSM模块，模块的名称为 network.
- step3: 假设传递过来的参数为 "ip:11.22.33.44", 此时这个信息优先级最高,会覆盖配置文件中的配置
    - step2.1: Convert API String to Cluster(Default in Session).vim 转换，从section中载入同名的key。ip信息从参数中载入; port由于参数中没有提供，但是network中定义了port为8080，所以最后Cluster中port为8080.
    - step2.2: Convert API String to Cluster(Default in Session).vim 转换，从给定的section/key载入配置。ip信息从参数中载入; port由于参数中没有提供，但是network.address1中定义了port为8081，所以最后Cluster中port为8081.
- step4: 假设传递过来的参数为空"", 相当于没有提供参数，此时有限采用配置文件中的配置
    - step4.1: Convert API String to Cluster(Default in Session).vim 转换，从section中载入同名的key。ip信息使用network.ip，结果是10.144.41.41； port使用network.port,结果时 8080.
    - step4.2: Convert API String to Cluster(Default in Session).vim 转换，从给定的section/key载入配置。从 network.address1 中载入配置，结果为 ip:10.144.42.42;port:8081.
- step5: 假设传递过来的参数为空"", 相当于没有提供参数; 同时ini配置文件中也没有定义对应的配置，此时采用默认常量参数。
    - step5.1: Convert API String to Cluster(Default in Session).vim 转换，"non-existing module" section 不存在，使用提供的参考数据. 结果为 ip:127.0.0.1;port:80.
    - step5.2: Convert API String to Cluster(Default in Session).vim 转换，从给定的section/key载入配置, 但是 section/key都不存在。使用提供的参考数据. 结果为 ip:127.0.0.1;port:80.