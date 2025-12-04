# CSM INI-Variable Addon

> [!NOTE] CSM INI-Variable Addon
>
> 配置文件是应用程序开发中不可或缺的组成部分。CSM INI-Variable Addon 为 CSM 提供简单易用的配置文件支持功能，使用户能够配置应用程序而无需显式读写配置文件。
>
> CSM INI-Variable 配置文件数据的格式，使用 CSM API String 格式。
>
> 主要特点包括：
> 1. **默认配置处理**：首次调用库函数时自动加载默认配置文件，无需用户显式加载。
> 2. **多文件支持**：通过专用函数支持加载多个配置文件。
> 3. **内存缓存**：在内存中维护一个缓存副本，应用程序从该缓存中获取配置信息。
> 4. **INI 格式兼容**：配置文件和内存副本均采用标准 INI 格式，支持节和键值对。
> 5. **高效缓存机制**：使用全局修改标记优化性能，仅在配置发生修改时才重新读取内存副本。
> 
> 本库包含并使用了由 [@rcpacini](https://github.com/rcpacini) 开发的 [LabVIEW-Config](https://github.com/rcpacini/LabVIEW-Config) 的副本。


> [!NOTE] CSM INI-Variable 变量格式
>
> 格式定义: ${section.variable:defaultValue}>
> - `${}`: 变量引用语法。
> - `section`（可选）: 配置文件中的节名。
> - `variable`: 配置文件中的变量名。
> - `defaultValue`（可选）: 默认值，当变量不存在时使用。
>
> 说明：
> - 支持嵌套的变量引用，例如 `${section1.variable1:${section2.variable2:defaultValue2}}`。
> - section 参数为可选。省略时，使用默认配置段 `SectionName=LabVIEW`。
> - 默认值为可选，未指定时默认为空字符串（""）。


> [!NOTE] CSM INI-Variable 配置文件路径
>
> - 开发状态：Application Directory 中找到的第一个 INI 配置文件。若不存在配置文件，则默认为 `csm-app.ini`。
> - 编译后：可执行文件所在目录中与可执行文件同名的 INI 配置文件（LabVIEW 编译后会自动生成此文件）。
> - 支持载入多个配置文件，后加载的文件会覆盖先前加载文件中的相同配置项。
> - 配置文件中可以使用 `[__include]` 节引用其他配置文件
>   - 可以使用相对路径，也可以使用绝对路径。
>   - 使用相对路径时，相对于当前配置文件的路径。
>   - [__include] 节中的配置文件名称不重要，只需要确保路径正确即可。
>   - 为了避免循环引用，同一个配置文件，第二次加载时会自动忽略。


> [!NOTE] CSM INI-Variable 多文件配置场景
>
> - 加载多个文件时，后加载的文件会覆盖先前加载文件中的相同配置项。
> - 将缓存更改保存到文件时，修改会保存到最后加载的配置文件中。


> [!WARNING] CSM INI-Variable 缓存机制警告
> 请注意，由于本库使用全局缓存修改标志，频繁的配置更改会降低读取 VI 中缓存机制的有效性。因此，本库不建议用于需要频繁修改配置的场景。

## CSM - Populate Configuration Variables.vi

在字符串中填充配置变量.

应用场景：在CSM的脚本中直接使用INI-Variable变量，或充当CSM消息的参数。

参考范例：1. Used as parameters parsed by CSM.vi

> Ref: CSM INI-Variable Addon
> Ref: CSM INI-Variable 变量格式
> Ref: CSM INI-Variable 配置文件路径

-- <b>输入控件 (Controls)</b> --
- <b>Section Name ("" as Default)</b>: 节名，空字符串表示使用默认节。
- <b>String with INI-Variable</b>: 包含变量引用的输入字符串。

-- <b>输出控件 (Indicators)</b> --
- <b>String</b>: 填充变量后的输出字符串。

## CSM - Read Configuration Variable.vim

根据原型，读取配置中的变量值。

- 推荐使用簇作为配置原型，族中的元素名称对应INI配置文件中的变量名。
- 如果是其他数据类型，对应的变量名称为"Config"。
- 如果原型对应的变量在配置中不存在，则根据 Write If not found? 参数确定是否写入默认值。

应用场景: 直接将INI配置文件中的配置读取成LabVIEW数据。

参考范例：2. Load the corresponding configuration by providing the prototype.vi

> Ref: CSM INI-Variable 配置文件路径

-- <b>输入控件 (Controls)</b> --
- <b>Configuration Prototype (Cluster Prefered)</b>: 配置原型（推荐使用簇）。
- <b>Section Name ("" as Default)</b>: 节名，空字符串表示使用默认节。
- <b>Write If not found? (F)</b>: 如果未找到，是否写入默认值。

-- <b>输出控件 (Indicators)</b> --
- <b>Configuration</b>: 输出配置值。

## CSM - Read Cluster Elements From Session.vim

读取配置中的簇数据，数据保存在指定的节(section)中, 簇中的元素名称对应节(section)中的键(key)。

- 如果指定的节(section)不存在，则创建使用默认节。
- 如果簇中的元素对应的键(key)不存在，则使用输入原型中的值作为默认值

应用场景: 直接将INI配置文件中的配置读取成LabVIEW数据。

参考范例：2. Load the corresponding configuration by providing the prototype.vi

> Ref: CSM INI-Variable 配置文件路径

-- <b>输入控件 (Controls)</b> --
- <b>Cluster</b>: 簇原型。
- <b>Section Name ("" as Default)</b>: 节名，空字符串表示使用默认节。

-- <b>输出控件 (Indicators)</b> --
- <b>Cluster out</b>: 输出簇。

## CSM - Read Cluster Elements From Key.vim

读取配置中的簇数据，数据保存在指定的节(section)中的指定键(key),数据以API String格式保存。

应用场景: 直接将INI配置文件中的配置读取成LabVIEW数据。

参考范例：2. Load the corresponding configuration by providing the prototype.vi

> Ref: CSM INI-Variable 配置文件路径

-- <b>输入控件 (Controls)</b> --
- <b>Cluster</b>: 簇原型。
- <b>Key</b>: 键名。
- <b>Section Name ("" as Default)</b>: 节名，空字符串表示使用默认节。

-- <b>输出控件 (Indicators)</b> --
- <b>Cluster out</b>: 输出簇。
- <b>Key Found?</b>: 键是否存在。

## Convert API String to Cluster(Default in Session).vim

将 API 字符串转换为簇（默认从会话中读取）。

-- <b>输入控件 (Controls)</b> --
- <b>API String</b>: API 字符串。
- <b>Section Name ("" as Default)</b>: 节名，空字符串表示使用默认节。
- <b>Type</b>: 数据类型。

-- <b>输出控件 (Indicators)</b> --
- <b>Data</b>: 转换后的数据。
- <b>error</b>: 错误信息。

## Convert API String to Cluster(Default in Key).vim

将 API 字符串转换为簇（默认从键中读取）。

-- <b>输入控件 (Controls)</b> --
- <b>API String</b>: API 字符串。
- <b>Section Name ("" as Default)</b>: 节名，空字符串表示使用默认节。
- <b>Type</b>: 数据类型。
- <b>Variable Name</b>: 变量名称。

-- <b>输出控件 (Indicators)</b> --
- <b>Data</b>: 转换后的数据。
- <b>error</b>: 错误信息。

## CSM - Write Configuration Variable.vim

写入配置变量。

- 推荐使用簇作为配置原型，族中的元素名称对应INI配置文件中的变量名。
- 如果是其他数据类型，对应的变量名称为"Config"。

参考范例：7. Write and Read Configuration.vi

> Ref: CSM INI-Variable 配置文件路径
> Ref: CSM INI-Variable 缓存机制警告

-- <b>输入控件 (Controls)</b> --
- <b>Configuration</b>: 要写入的配置数据。
- <b>Section Name ("" as Default)</b>: 节名，空字符串表示使用默认节。

-- <b>输出控件 (Indicators)</b> --
- <b>Configuration (dup)</b>: 写入的配置数据副本。

## 原始配置信息读取

### CSM - Read All Variable Names.vi

读取当前内存中所有变量名称。

-- <b>输入控件 (Controls)</b> --
- <b>Permanent Variables Only? (F)</b>: 是否仅读取永久变量。

-- <b>输出控件 (Indicators)</b> --
- <b>All Variable Names</b>: 变量名称数组。

### CSM - Read Sections.vi

读取当前内存中所有节名称。

-- <b>输出控件 (Indicators)</b> --
- <b>Sections</b>: 节名称数组。

### CSM - Read Key Names.vi

读取指定节中的所有键名。

-- <b>输入控件 (Controls)</b> --
- <b>Section Name ("" as Default)</b>: 节名，空字符串表示使用默认节。

-- <b>输出控件 (Indicators)</b> --
- <b>Keys</b>: 键名数组。

### CSM - Read INI String.vi

读取配置信息字符串。此读取为获取原始的字符串描述，不替换INI变量引用。

应用场景：需要读取原始的配置信息，而不是替换内部变量引用后的配置信息。

参考范例：6. Read Nested Variables.vi

-- <b>输入控件 (Controls)</b> --
- <b>Section Name ("" as Default)</b>: 节名，空字符串表示使用默认节。
- <b>Key</b>: 包含变量引用的输入字符串。
- <b>Default Value</b>: 默认值。
- <b>Write If not found? (F)</b>: 如果未找到，是否写入默认值。

-- <b>输出控件 (Indicators)</b> --
- <b>Value</b>: 替换变量后的输出字符串。
- <b>Key Found?</b>: 键是否存在。

### CSM - Write INI String.vi

写入配置信息字符串。

参考范例：6. Read Nested Variables.vi

> Ref: CSM INI-Variable 缓存机制警告

-- <b>输入控件 (Controls)</b> --
- <b>Section Name ("" as Default)</b>: 节名，空字符串表示使用默认节。
- <b>Key</b>: 键名。
- <b>Value</b>: 变量值字符串。

-- <b>输出控件 (Indicators)</b> --
- <b>Key Replace?</b>: 键是否被替换。

## 多文件支持

### CSM - Configuration File Path.vi

返回加载的所有配置文件路径。

参考范例：5. import Config.ini with __include section.vi

> Ref: CSM INI-Variable 配置文件路径

-- <b>输入控件 (Controls)</b> --
- <b>Include All Paths?(F)</b>: 是否包含所有配置文件路径。

-- <b>输出控件 (Indicators)</b> --
- <b>Default Config File Path</b>: 默认配置文件路径。
- <b>All Config Files</b>: 所有配置文件列表。

### CSM - Load Configuration Variables From File.vi

从指定文件加载配置变量。如果节名后缀名不为空，载入的节名称会加上后缀。

<b>Section Postfix ("")</b>主要用于载入多个配置文件，但具有相同节的情况。例如：两个配置文件：Hardware1.ini 和 Hardware2.ini，两个配置文件都有一个名为 "Serial" 的节, 并且内部的Keys 相同，表示串口配置，那么后载入的文件中的配置会覆盖先载入的文件中的配置。使用<b>Section Postfix ("")</b>来避免节名冲突。

参考范例：5. import Config.ini with __include section.vi

> Ref: CSM INI-Variable 配置文件路径

-- <b>输入控件 (Controls)</b> --
- <b>Configuration Path</b>: 配置文件路径。
- <b>Section Postfix ("")</b>: 节名后缀。

### CSM - Mark All Temp Variables as Permanent.vi

将所有临时变量标记为永久变量，并将变量存储到指定的配置文件中。

注意：虽然已经标记为永久变量，但是如果不使用 CSM - Sync Configuration Variables to File.vi, 依然不会将变量同步到文件中。

> Ref: CSM INI-Variable 配置文件路径

-- <b>输入控件 (Controls)</b> --
- <b>Path ("" to Use Default File)</b>: 配置文件路径，空字符串表示使用默认 INI 文件。

### CSM - Sync Configuration Variables to File.vi

将内存中的变量同步到指定的配置文件中。

注意：只有永久变量才会同步到文件中，如果想将临时变量写入文件，需要提前将临时变量标记为永久变量。

> Ref: CSM INI-Variable 配置文件路径

-- <b>输入控件 (Controls)</b> --
- <b>All Temp Variable to Default Ini(F)</b>: 是否将所有临时变量同步到默认 INI 文件。
- <b>Write UnSupported Datatype?(F)</b>: 是否写入不支持的数据类型。

### CSM - Unload Configuration Variable File.vi

卸载配置变量文件。(@wenjia 补充行为)

> Ref: CSM INI-Variable 配置文件路径

-- <b>输入控件 (Controls)</b> --
- <b>Path</b>: 配置文件路径。

### CSM - Restore Variable Value.vi

恢复变量值, 将所有的永久变量刷新为首次载入的配置值，临时变量不会被修改。

> Ref: CSM INI-Variable 配置文件路径

### CSM - Reset to Default.vi

将配置重置为默认值。所有的变量将被清空，所有的配置会从现在加载的配置文件中重新加载。

> Ref: CSM INI-Variable 配置文件路径

## CSM - Read Log Filter Rules from INI Strings.vi

从 INI 字符串中读取日志过滤规则。

## CSM - Read File Logger Configuration from INI String.vi

从 INI 字符串中读取文件记录器配置。


## Read CSM Log Filter Rules.vi

读取 CSM 日志过滤规则。


## CSM - Read File Logger Configuration - v1.0.vi

读取文件记录器配置（v1.0）。

-- <b>输入控件 (Controls)</b> --
- <b>Section Name("FileLogger")</b>: 节名，默认为 "FileLogger"。

-- <b>输出控件 (Indicators)</b> --
- <b>FileLogger Configuration-v1</b>: 文件记录器配置。

## CSM - Read Log Filter Rules - v1.0.vi

读取日志过滤规则（v1.0）。

-- <b>输入控件 (Controls)</b> --
- <b>Section Name("LogFilterRules")</b>: 节名，默认为 "LogFilterRules"。

-- <b>输出控件 (Indicators)</b> --
- <b>Rules-v1</b>: 日志过滤规则。

