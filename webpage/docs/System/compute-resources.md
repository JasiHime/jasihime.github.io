#Compute Resources

There are four types of nodes in the MARS cluster for processing different workloads!

|CPU Nodes||
|---------|-|
|Count|10|
|Specifications|2x AMD 7543 Processors @2.8Ghz 32 cores|
||512Gb RAM|
|Partition|nodes|
|Hostnames|`node01 – 10`|

|CPU+ Nodes||
|---|---|
|Count|6|
|Specifications|2x AMD 7763 Processors @2.45Ghz 64 cores|
||1Tb RAM|
|Partition7smp|
|Hostnames|`node101 – 106`|

|GPU Nodes||
|---|---|
|Count|20|
|Specifications|2x AMD 7543 Processors @2.8Ghz|
||256Gb RAM|
||Nvidia A40 (48GB)|
|Partition|gpu|
|Hostnames|`gpu01 – 20`|

|GPU+ Nodes||
|---|---|
|Count|4|
|Specifications|2x AMD 7763 Processors @2.8Ghz|
||512Gb RAM|
||Nvidia HGX – 4x A100 GPU (80GB)|
|Partition|gpuplus|
|Hostnames|`gpu101 – 104`|

\* GPU+ resources can only be used as part of a project with GPU+ permission. Please specify the need for these resources in your project application.
