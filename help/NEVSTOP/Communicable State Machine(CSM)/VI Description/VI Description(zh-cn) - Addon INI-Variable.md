# CSM INI-Variable Addon

## 概述

> [!NOTE] CSM Massdata 参数支持
> CSM Massdata 参数支持提供了一种高效的参数传递机制，用于在 CSM 中传递大量数据。通过使用内存高效的引用机制而非直接传输数据，从而提高了参数传递的效率和性能。
> - CSM Massdata 定义的参数类型为 `<MassData>`，可以通过 `## CSM - Argument Type.vi` 获取。
> - CSM Massdata 数据传输是完全无损的，但不适合存储需要无限期持久化的数据。
> - 注意：Massdata Support 内部使用循环缓冲区机制，同一应用程序内的所有 CSM 模块共享相同的 Massdata 缓冲区空间。当缓冲区满时，新数据将从开始位置覆盖旧数据。因此需要设置合理的缓冲区大小，确保数据在使用完毕前未被覆盖，避免数据丢失。

> [!NOTE] CSM Massdata 参数格式
> 典型的 massdata 数据格式为: `<MassData>Start:8057;Size:4004;DataType:1D I32`。其中：
> - `Start`: 数据在内存中的起始地址
> - `Size`: 数据的大小（字节数）
> - `DataType(optional)`: 数据的类型，由 `CSM Data Type String.vi` 定义

## CSM - Configuration File Path.vi

- <b>All Config Files</b>:Indicators
- <b>Default Config File Path</b>:Indicators
- <b>Include All Paths?(F)</b>:Controls

## CSM - Load Configuration Variables From File.vi

- <b>Path</b>:Controls
- <b>section postfix("")</b>:Controls

## CSM - Mark All Temp Variables as Permanent.vi

- <b>Path("" to use default ini)</b>:Controls

## CSM - Populate Configuration Variables.vi

- <b>Section Name("" as Default)</b>:Controls
- <b>String in</b>:Controls
- <b>String out</b>:Indicators

## CSM - Read All Variable Names.vi

- <b>Array</b>:Indicators
- <b>Permanent Variables Only?</b>:Controls

## CSM - Read Cluster Elements From Key.vim

- <b>Cluster out</b>:Indicators
- <b>Cluster</b>:Controls
- <b>Key Found?</b>:Indicators
- <b>Key</b>:Controls
- <b>Section Name("" as Default)</b>:Controls

## CSM - Read Cluster Elements From Session.vim

- <b>Cluster out</b>:Indicators
- <b>Cluster</b>:Controls
- <b>Section Name("" as Default)</b>:Controls

## CSM - Read Configuration Variable.vim

- <b>Section Name("" as Default)</b>:Controls
- <b>Write If not found?(F)</b>:Controls
- <b>config out</b>:Indicators
- <b>config prototype(Cluster Prefered)</b>:Controls

## CSM - Read File Logger Configuration from INI String.vi



## CSM - Read INI String.vi

- <b>>> Default Value >></b>:Controls
- <b>Key Found?</b>:Indicators
- <b>Section Name("" as Default)</b>:Controls
- <b>String in</b>:Controls
- <b>String out</b>:Indicators
- <b>Write If not found?(F)</b>:Controls

## CSM - Read Key Names.vi

- <b>Array</b>:Indicators
- <b>Key Found?</b>:Indicators
- <b>Section Name("" as Default)</b>:Controls

## CSM - Read Log Filter Rules from INI Strings.vi



## CSM - Read Sessions.vi

- <b>Array</b>:Indicators
- <b>Key Found?</b>:Indicators

## CSM - Reset to Default.vi



## CSM - Restore Variable Value.vi



## CSM - Sync Configuration Variables to File.vi

- <b>All Temp Variable to Default Ini(F)</b>:Controls
- <b>Write UnSupported Datatype?(F)</b>:Controls

## CSM - Unload Configuration Variable File.vi

- <b>Path</b>:Controls

## CSM - Write Configuration Variable.vim

- <b>Section Name("" as Default)</b>:Controls
- <b>Variant (dup)</b>:Indicators
- <b>Variant</b>:Controls

## CSM - Write INI String.vi

- <b>>> Variable Name >></b>:Controls
- <b>Key Replace?</b>:Indicators
- <b>Section Name("" as Default)</b>:Controls
- <b>String</b>:Controls

## Convert API String to Cluster(Default in Key).vim

- <b>API String</b>:Controls
- <b>Data</b>:Indicators
- <b>Section Name("" as Default)</b>:Controls
- <b>Type</b>:Controls
- <b>Variable Name</b>:Controls
- <b>error</b>:Indicators

## Convert API String to Cluster(Default in Session).vim

- <b>API String</b>:Controls
- <b>Data</b>:Indicators
- <b>Section Name("" as Default)</b>:Controls
- <b>Type</b>:Controls
- <b>error</b>:Indicators

## Default Ini Config File.vi

- <b>appended path</b>:Indicators







## Import Permanent Variable From File.vi

- <b>CSM-Static-Variable-Manager in</b>:Controls
- <b>CSM-Static-Variable-Manager out</b>:Indicators
- <b>Path</b>:Controls
- <b>section postfix</b>:Controls

Is HexStr Content.vi

- <b>Content</b>:Controls
- <b>HexStr Content</b>:Indicators
- <b>hexStr</b>:Indicators

Key Information From Index.vi

- <b>Config in</b>:Controls
- <b>Config out</b>:Indicators
- <b>Key Information</b>:Indicators
- <b>Key Name</b>:Controls
- <b>Section Name</b>:Controls
- <b>Start</b>:Controls

Key Information.vi

- <b>Config in</b>:Controls
- <b>Config out</b>:Indicators
- <b>Key Information</b>:Indicators
- <b>Key Name</b>:Controls
- <b>Section Name</b>:Controls

List Files.vi

- <b>CSM-Static-Variable-Manager in</b>:Controls
- <b>CSM-Static-Variable-Manager out</b>:Indicators
- <b>File List</b>:Indicators

List Keys.vi

- <b>Array</b>:Indicators
- <b>CSM-Static-Variable-Manager in</b>:Controls
- <b>CSM-Static-Variable-Manager out</b>:Indicators
- <b>Key?</b>:Indicators
- <b>Section Name</b>:Controls

List Permanent Variables.vi

- <b>CSM-Static-Variable-Manager in</b>:Controls
- <b>CSM-Static-Variable-Manager out</b>:Indicators
- <b>Permanent Variables</b>:Indicators

List Sessions.vi

- <b>Array</b>:Indicators
- <b>CSM-Static-Variable-Manager in</b>:Controls
- <b>CSM-Static-Variable-Manager out</b>:Indicators

List Variables.vi

- <b>CSM-Static-Variable-Manager in</b>:Controls
- <b>CSM-Static-Variable-Manager out</b>:Indicators
- <b>Variables</b>:Indicators

Mark All Temp Variables as Permanent.vi

- <b>CSM-Static-Variable-Manager in</b>:Controls
- <b>CSM-Static-Variable-Manager out</b>:Indicators
- <b>Path</b>:Controls
- <b>SyncRef?(F)</b>:Controls

MultipleLines to String.vi

- <b>Array</b>:Controls
- <b>String</b>:Indicators

New.vi

- <b>Config</b>:Indicators
- <b>Path ("")</b>:Controls
- <b>Text</b>:Controls

Open.vi

- <b>Config</b>:Indicators
- <b>Create if not exists? (T)</b>:Controls
- <b>File Created?</b>:Indicators
- <b>File Exists?</b>:Indicators
- <b>Path</b>:Controls

Parse Contained Variables.vi

- <b>Contained Variables</b>:Indicators
- <b>String</b>:Controls

Parse Token.vi

- <b>Line</b>:Controls
- <b>Name</b>:Indicators
- <b>Token</b>:Controls
- <b>Value</b>:Indicators

Parse Used Session and Name.vi

- <b>Section Name("" as Default)</b>:Controls
- <b>name</b>:Controls
- <b>used name</b>:Indicators

Parse Value.vi

- <b>Value (Comment Removed)</b>:Indicators
- <b>Value (Quotes Removed)</b>:Indicators
- <b>Value</b>:Controls

Populate String with INI Variable.vi

- <b>Section</b>:Controls
- <b>String might with variables</b>:Controls
- <b>String out</b>:Indicators
- <b>depth</b>:Controls
- <b>variable History</b>:Controls




Read CSM Log Filter Rules.vi



Read INI String.vi

- <b>CSM-Static-Variable-Manager in</b>:Controls
- <b>CSM-Static-Variable-Manager out</b>:Indicators
- <b>Content</b>:Indicators
- <b>Default</b>:Controls
- <b>Force Default Session?(F)</b>:Controls
- <b>Key Found?</b>:Indicators
- <b>Section Name</b>:Indicators
- <b>Variable</b>:Controls





Restore Variable Value.vi

- <b>CSM-Static-Variable-Manager in</b>:Controls
- <b>CSM-Static-Variable-Manager out</b>:Indicators

Session Information.vi

- <b>Config in</b>:Controls
- <b>Config out</b>:Indicators
- <b>Section Name</b>:Controls
- <b>Session Information</b>:Indicators

String Escape.vi

- <b>Escaped String</b>:Indicators
- <b>Unescaped String</b>:Controls

String Unescape.vi

- <b>Escaped String</b>:Controls
- <b>Unescaped String</b>:Indicators

Sync to File.vi

- <b>CSM-Static-Variable-Manager in</b>:Controls
- <b>CSM-Static-Variable-Manager out</b>:Indicators
- <b>Write Reference?(F)</b>:Controls


Write INI String.vi

- <b>CSM-Static-Variable-Manager in</b>:Controls
- <b>CSM-Static-Variable-Manager out</b>:Indicators
- <b>Content</b>:Controls
- <b>Force Default Session?(F)</b>:Controls
- <b>Key Replaced?</b>:Indicators
- <b>Variable</b>:Controls



## CSM - Read File Logger Configuration - v1.0.vi

- <b>FileLogger Configuration-v1</b>:Indicators
- <b>Section Name("FileLogger")</b>:Controls

_## CSM - Read Log Filter Rules - v1.0.vi

- <b>Rules-v1</b>:Indicators
- <b>Section Name("LogFilterRules")</b>:Controls

