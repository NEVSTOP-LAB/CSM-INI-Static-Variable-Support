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

## 多文件配置系统(4. Multi-file configuration system.vi)

### overview

本范例展示 CSM INI-Variable 对于多文件配置的支持。

### Introduction

本范例展示 CSM INI-Variable 对于多文件配置的支持。利用多文件配置功能，可实现分布式配置文件系统。

CSM INI-Variable Support 具有一个默认的配置文件，无需显示加载，会在程序实例启动时后台自动载入。
    - 开发状态：Application Directory 中找到的第一个 INI 配置文件。若不存在配置文件，则默认为 csm-app.ini。
    - 编译后：可执行文件所在目录中与可执行文件同名的 INI 配置文件（LabVIEW 编译后会自动生成此文件）。

加载多个文件时，后加载的文件会覆盖先前加载文件中的相同配置项。将缓存更改保存到文件时，修改会保存到最后加载的配置文件中。

### steps

- step1: 生成一个临时的INI文件，使用 CSM - Load Configuration Variables From File.vi 加载该文件。
- step2: 再生成一个临时的INI文件，使用 CSM - Load Configuration Variables From File.vi 加载该文件，需要注意的是，RS232_Device section中的 resource key 会被覆盖。
- step3: 应用场景展示
    - step3.1: 使用 CSM - Read Cluster Elements From Session.vim, 读取 Cluster 配置。
    - step3.2: 再参数固化中使用，可以看出，无需发送串口的信息，"SerialPort Initialize" 中会使用配置文件中的信息初始化串口
    - step3.3: step3.2中如果发送了串口信息，以发送的信息为准，"SerialPort Initialize" 中会使用发送的信息初始化串口。

## 使用 [__include] 引用配置文件(5. import Config.ini with __include section.vi)

### Overview

本范例展示 CSM INI-Variable 对于引用配置文件的支持。利用引用配置文件功能，可实现分布式配置文件系统。

### Introduction

CSM INI-Variable Support 载入的配置文件中，若包含 [__include] 段落，会自动引用指定的配置文件。需要注意的是：

- 如果是相对路径，会相对于当前配置文件的路径进行解析；如果是绝对路径，会直接使用该路径。
- 请注意避免循环依赖，否则可能导致无限循环。CSM INI-Variable Support 会维护已加载配置文件的记录，当第二次尝试加载同一文件时，将跳过该次加载操作。
- [__include] 中的key名称不重要，只作为一个标识, value中的路径才是重要的。
- [__include] 中的配置信息会先被载入，类似多文件加载时先加载，因此当前配置文件中的配置项会覆盖引用文件中的相同配置项。

### steps

- step1: 创建多个配置文件，注意内部引用关系。另外注意 [__include] 大消息不敏感。
- step2: 使用 CSM - Load Configuration Variables From File.vi 加载该文件
- step3: 可以使用 CSM - Configuration File Path.vi 获取当前加载的所有配置文件路径。
- step4: 尝试读取配置信息，请注意覆盖关系后，实际生效的配置。

## 嵌套变量(6. Read Nested Variables.vi)

### Overview

CSM INI-Static-Variable-Support 支持嵌套变量解析，允许在键中引用其他键，实现更灵活的配置定义。格式遵循 ${section.variable:defaultValue}。本范例展示了如何在配置文件中定义和引用嵌套变量。

### Introduction

CSM INI-Static-Variable-Support 支持嵌套变量解析，允许在键中引用其他键，实现更灵活的配置定义。格式遵循 ${section.variable:defaultValue}。

- 读取 API 行为：CSM INI Read String.vi 获取原始配置值而不解析嵌套变量。其他所有读取 API 都会自动解析嵌套变量。
- 写入 API 行为：所有写入 API 都会直接覆盖配置值。通常情况下，包含嵌套引用的键不应通过写入操作直接修改。

举例：

```
// 默认配置
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

使用上面的配置文件示例：

场景1：读取 ${file.path} 会返回一个由其他配置项动态组合而成的实际文件路径，从而实现灵活的路径定义。
场景2：[case1] 和 [case2] 节定义了两组不同的相关配置信息。通过修改 ${RT.select}，您可以切换访问 ${RT.addr} 的结果。

### steps

- step1: 生成一个临时的INI文件，使用 CSM - Load Configuration Variables From File.vi 加载该文件。
- step2: 嵌套变量中引用同一个section的配置
    - step2.1 CSM - Read INI String 加载的是原始配置，不会解析嵌套变量
    - step2.2 CSM - Read Configuration Variable.vim 会解析嵌套变量
    - step2.3 CSM - Populate Configuration Variables.vi  会解析嵌套变量
- step3: 嵌套变量中引用的其他section的配置, 当指明section时，使用指定的section,否则优先使用当前section的配置
- step4: 展示嵌套变量中变量名也可以引用的情况，例如：${case${select}.addr}
    - step4.1 修改 ${RT.select} 为 2
    - step4.2 读取 ${RT.addr} 会返回 ${case2.addr}
    - step4.3 (optional) 手动修改 修改 ${RT.select} 为 1, 再次运行，查看配置信息的变化
- step5: 展示使用不同字段信息拼接产品路径的场景，例如定义了 file.path = ${root}/${info.operator}/${info.date}/${info.test}${info.time}.tdms
    - step5.1 通过修改 info.date 和 info.time, 可以发现 file.path 实际解析的路径会根据配置信息动态变化

