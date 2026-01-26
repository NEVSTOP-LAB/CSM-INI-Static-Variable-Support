# CSM INI-Variable Support

## CSM Parsable Parameters (1. Used as parameters parsed by CSM.vi)

### Overview

CSM INI Variable Support provides CSM with simple and easy-to-use configuration file support, enabling users to configure applications without explicitly reading or writing configuration files.

This example demonstrates how to use the INI Variable Support parsable parameters feature within CSM.

### Introduction

This example shows how to use the CSM INI Variable Support feature to store parameter information in an INI configuration file. INI variables use the CSM API String format.

Variable Syntax: `${section.variable:defaultValue}`

- **section:** The section name in the configuration file. The `section` parameter is optional. If omitted, the default configuration section `SectionName=LabVIEW` is used.
- **variable:** The variable name in the configuration file.
- **defaultValue:** The default value used when the variable is not defined in the configuration file. The `defaultValue` is optional. If unspecified, it defaults to an empty string (`""`).

### Steps

- Step1: Generate a temporary INI file and load it using the CSM - Load Configuration Variables From File VI.

- Step2: Simulate a CSM module using a regular loop and use the CSM - Populate Configuration Variables VI to parse variables within a string into actual values. After running, compare the information displayed on the front panel with the comments to verify consistency.

  - Step2.1: In the code for this part, note that the section/variable does not exist in the configuration information.
  - Step2.2: Expected result for the code execution in Step2.1.
  - Step2.3: In the code for this part, note that CSM INI Variable Support is not limited to parameters. It can be used with any field.
  - Step2.4: Expected result for the code execution in Step2.3.

  

## Loading Configuration by Providing a Cluster Prototype (2. Load the corresponding configuration by providing the prototype.vi)

### Overview

Demonstrates loading configuration by providing a Cluster prototype. You can load configuration from an entire section or a specific key.

### Introduction

You can provide a Cluster prototype to load configuration.

Functions supporting this feature:

- **CSM - Read Cluster Elements From Session.vim:** Element names in the Cluster prototype are used as variable names in the configuration file.
- **CSM - Read Cluster Elements From Key.vim:** Requires a Key parameter. The cluster uses the API String format stored in the given section/key.

**Special Case Note:**

- If the variable is not defined in the configuration file, the element data from the provided prototype Cluster will be used.

### Steps

- Step1: Generate a temporary INI file and load it using `CSM - Load Configuration Variables From File.vi`.
- Step2: Load configuration using `CSM - Read Cluster Elements From Session.vim`.
- Step3: Use `CSM - Populate Configuration Variables.vi` to parse variables within strings into actual values.



## Persisting CSM API Parameters (3. In CSM API parameters.vi)

### Overview

Demonstrates how to use the CSM INI Variable Support feature to persist parameter information in an INI configuration file.

### Introduction

This example shows how to persist parameter information in an INI configuration file using CSM INI Variable Support API, primarily through `Convert API String to Cluster(Default in Session).vim` or `Convert API String to Cluster(Default in Key).vim`. They function identically to their counterparts in CSM API String Support but add the functionality of reading INI configuration files.

Data sources may include: information sent via message parameters, parameters in the configuration file, or default constant parameters provided at design time. They have a clear priority hierarchy: CSM API Parameters > Configuration File Parameters > Default Constant Parameters.

**Notes:**

- During initialization, explicitly sent parameters have the highest priority.
- If no parameters are sent, parameters from the configuration file are used.
- In the absence of configuration file parameters, default constant parameters are applied.
- Parameters can be partially specified. Missing values are automatically filled using configuration information from the next priority level.

### Steps

- Step1: Generate a temporary INI file and load it using `CSM - Load Configuration Variables From File.vi`.

- Step2: Simulate a CSM module named "network" using a standard loop.

- Step3: Assume the passed parameter is "ip:11.22.33.44". This information has the highest priority and overrides the configuration in the INI file.

  - Step3.1: `Convert API String to Cluster(Default in Session).vim` conversion loads the key with the same name from the section. The IP information is loaded from the parameter. The port is not provided in the parameter, but port is defined as 8080 in "network". So the final cluster port is 8080.
  - Step3.2: `Convert API String to Cluster(Default in Session).vim` conversion loads configuration from the given section/key. The IP information is loaded from the parameter. The port is not provided in the parameter, but port is defined as 8081 in network.address1. Thus the final cluster port is 8081.

- Step4: Assume the passed parameter is empty `""`, equivalent to providing no parameter. In this case, the configuration from the file is prioritized.

  - Step4.1: `Convert API String to Cluster(Default in Session).vim` conversion loads the key with the same name from the section. The IP information uses network.ip, resulting in IP: 10.144.41.41. The port uses network.port, resulting in 8080.
  - Step4.2: `Convert API String to Cluster(Default in Session).vim` conversion loads configuration from the given section/key. Configuration is loaded from network.address1, resulting in IP:10.144.42.42 and port:8081.

- Step5: Assume the passed parameter is empty `""`, equivalent to providing no parameter. The corresponding configuration is not defined in the INI configuration file. In this case, default constant parameters are used.

  - Step5.1: `Convert API String to Cluster(Default in Session).vim` conversion. The "non-existing module" section does not exist, so the provided reference data is used, resulting in IP:127.0.0.1 and port:80.
  - Step5.2: `Convert API String to Cluster(Default in Session).vim` conversion. Load configuration from the given section/key, but neither exists. The provided reference data is used, resulting in IP:127.0.0.1 and port:80.

  

## Multi-file Configuration System (4. Multi-file configuration system.vi)

### Overview

Demonstrates using CSM INI Variable Support API for multi-file configuration.

### Introduction

This example shows using CSM INI Variable Support API for multi-file configuration. You can use the multi-file configuration feature to implement a distributed configuration file system.

CSM INI Variable Support API has a default configuration file that does not need to be explicitly loaded. The default configuration file automatically loads in the background when the application instance starts. You can find the default configuration file in one of the following locations:

- **Development Mode:** The first INI configuration file found in the Application Directory. If no configuration file exists, it defaults to `csm-app.ini`.
- **Post-Compilation:** The INI configuration file with the same name as the executable located in the executable's directory. LabVIEW automatically generates this file after compilation.

When loading multiple files, configuration items in subsequently loaded files overwrite identical items in previously loaded files. When saving cached changes to a file, modifications are saved to the last loaded configuration file.

### Steps

- **Step 1:** Generate a temporary INI file and load it using `CSM - Load Configuration Variables From File.vi`.
- **Step 2:** Generate another temporary INI file and load it using `CSM - Load Configuration Variables From File.vi`. Note that the resource key in the "RS232_Device" section will be overwritten.
- **Step 3:** Application scenario demonstration.
  - **Step 3.1:** Read cluster configuration using `CSM - Read Cluster Elements From Session.vim`.
  - **Step 3.2:** Used in parameter persistence. As you can see, without sending serial port information, `"SerialPort Initialize"` will initialize the serial port using information from the configuration file.
  - **Step 3.3:** If serial port information is sent in Step 3.2, the sent information takes precedence, and `"SerialPort Initialize"` will initialize the serial port using the sent information.



## Referencing Configuration Files with [__include] (5. import Config.ini with include section.vi)

### Overview

This example demonstrates CSM INI Variable Support API for referencing configuration files. You can use the reference configuration file feature to implement a distributed configuration file system.

### Introduction

In configuration files loaded by CSM INI Variable Support API, if a [__include] section is present, the specified configuration files will be automatically referenced.

**Notes:**

- Relative paths are resolved relative to the current configuration file's path. Absolute paths are used directly.
- Avoid circular dependencies, as they may lead to infinite loops. CSM INI Variable Support API maintains a record of loaded configuration files and skips loading if an attempt is made to load the same file a second time.
- The key names in [__include] are not important. They serve only as identifiers. The paths in the values are what matters.
- Configuration information in [__include] is loaded first, similar to loading first in a multi-file load scenario). Therefore, configuration items in the current configuration file will overwrite identical items in the referenced files.

### Steps

- Step1: Create multiple configuration files. Pay attention to internal reference relationships. Also note that [__include] is case-insensitive.
- Step2: Load the file using `CSM - Load Configuration Variables From File.vi`.
- Step3: Use `CSM - Configuration File Path.vi` to retrieve the paths of all currently loaded configuration files.
- Step4: Attempt to read configuration information, noting the actual effective configuration after considering overwrite relationships.



## Nested Variables (6. Read Nested Variables.vi)

### Overview

CSM INI Variable Support API supports nested variable parsing, allowing keys to reference other keys for more flexible configuration definitions. The format follows ${section.variable:defaultValue}. This example demonstrates how to define and reference nested variables in a configuration file.

### Introduction

CSM INI Variable Support API supports nested variable parsing, allowing keys to reference other keys.

- **Read API Behavior:** `CSM INI Read String.vi` retrieves the raw configuration value without parsing nested variables. All other read APIs automatically parse nested variables.
- **Write API Behavior:** All write APIs directly overwrite the configuration value. Generally, keys containing nested references should not be modified directly via write operations.

**Configuration File Example:**

```
// Default Configuration
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

Using the configuration file example above:

**Scenario 1:** Reading `${file.path}` returns an actual file path dynamically composed of other configuration items, enabling flexible path definitions. 

**Scenario 2:** The `[case1]` and `[case2]` sections define two different sets of related configuration information. By modifying `${RT.select}`, you can switch the result of accessing `${RT.addr}`.

### Steps

- Step1: Generate a temporary INI file and load it using `CSM - Load Configuration Variables From File.vi`.
- Step2: Nested variables reference values in the same section.
  - Step2.1: `CSM - Read INI String` loads the raw configuration and does not parse nested variables.
  - Step2.2: `CSM - Read Configuration Variable.vim` parses nested variables.
  - Step2.3: `CSM - Populate Configuration Variables.vi` parses nested variables.
- Step3: Reference configuration from other sections within nested variables. When a section is specified, the specified section is used. Otherwise, the current section's configuration is prioritized.
- Step4: Demonstrate that variable names in nested variables can also be references. For example, `${case${select}.addr}`.
  - Step4.1: Modify `${RT.select}` to `2`.
  - Step4.2: Reading `${RT.addr}` will return `${case2.addr}`.
  - Step4.3: (Optional) Manually modify `${RT.select}` to `1`. Run again and observe the change in configuration information.
- Step5: Demonstrate the scenario of concatenating product paths using different field information. For example, defining `file.path = ${root}/${info.operator}/${info.date}/${info.test}${info.time}.tdms`).
  - Step5.1: By modifying `info.date` and `info.time`, you can observe that the actually parsed path for `file.path` changes dynamically based on the configuration information.



## Modifying Configuration Information (7. Write and Read Configuration.vi)

### Overview

Demonstrates modifying configuration information using CSM INI Variable Support API.

### Introduction

CSM INI VariableSupport provides APIs for modifying configuration information. You can use these APIs to dynamically update key-value pairs in the configuration file. Note that to improve efficiency, CSM INI Variable Support uses a global cache modification flag in read functions. When the overall configuration has not changed, cached data is used for rapid access. Frequent configuration changes will reduce the effectiveness of the caching mechanism in Read VIs. Therefore, this library is not recommended for scenarios requiring frequent configuration modifications.

You can use `CSM - Write Configuration Variable.vi` and `CSM - Write INI String.vi` to modify configuration information.

**Notes:**

- By default, configurations modified in memory are not automatically synchronized to the configuration file. You need to call `CSM - Sync Configuration Variables to File.vi` to synchronize them to the file.
- Modified variables will only be preserved when synchronized to the file if the original configuration file defined those specific configuration items. If the original configuration file did not define certain items, the modified variables are saved only in memory by default and are not synchronized to the file.
- You can call `CSM - Mark All Temp Variables as Permanent.vi` to mark all temporary variables as permanent, ensuring that all variables are preserved when synchronized to the configuration file.