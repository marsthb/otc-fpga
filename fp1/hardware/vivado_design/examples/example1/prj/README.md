﻿# Example 1 Building Guide

---

[切换到中文版](./README_CN.md)

<div id="table-of-contents">
<h2>Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#sec-1">1. <b>Directory Structure</b></a></li>
<li><a href="#sec-2">2. <b>File and Folder Description</b></a></li>
<li><a href="#sec-3">3. <b>Building Description</b></a>
<ul>
<li><a href="#sec-3-1">3.1. <b>usr_prj_cfg File Configuration Description</b></a></li>
</ul>
<ul>
<li><a href="#sec-3-2">3.2. <b>build.sh Script Operation Instructions</b></a></li>
</ul>
<ul>
<li><a href="#sec-3-3">3.3. <b>schedule_task.sh Script Operation Instructions</b></a></li>
</ul>
<ul>
<li><a href="#sec-3-5">3.4. <b>AEI_Register.sh Script Operation Instructions</b></a></li>
</ul>
</li>
<li><a href="#sec-4">4. <b>Others</b></a></li>
</ul>
</li>
</div>
</div>

<a id="sec-1" name="sec-1"></a>

## Directory Structure

The structure of the prj folder of Example 1 is as follows:

- **prj/**
  - [build/](#sec-2-1)
  - [constraints/](#sec-2-2)
  - [build.sh](#sec-2-3)
  - README_CN.md (Chinese version of this document)
  - README.md (this document)
  - [schedule_task.sh](#sec-2-4)
  - [usr_prj_cfg](#sec-2-5)
  - [AEI_Register.sh](#sec-2-7)

<a id="sec-2" name="sec-2"></a>

## File and Folder Description

<a id="sec-2-1" name="sec-2-1"></a>

- build  
  This directory stores example building information.

<a id="sec-2-2" name="sec-2-2"></a>

- constraints  
  This directory stores the user-defined constraint information of example 1, which derives from **USR_CONSTRAINTS** in `usr_prj_cfg`.

<a id="sec-2-3" name="sec-2-3"></a>

- build.sh  
  This file is used to build a project. After users modify `usr_prj_cfg`, they can run the file to build a project. For details, see [build.sh Script Operation Instructions](#sec-3-2).

<a id="sec-2-4" name="sec-2-4"></a>

- schedule_task.sh  
  This file is used for project delay and scheduled building. For details, see [schedule_task.sh Script Operation Instructions](#sec-3-3).

<a id="sec-2-5" name="sec-2-5"></a>

- usr_prj_cfg  
  This file is used to configure user-defined information about the example 1 project. For details, see [usr_prj_cfg File Configuration Description](#sec-3-1).

<a id="sec-2-7" name="sec-2-7"></a>

- AEI_Register.sh  
  This command is used to complete the PR verification and generate and upload the .bit file. Run this file to upload the .bit file to the OBS bucket and obtain the registration ID. For details, see [AEI_Register.sh Script Operation Instructions](#sec-3-5).

<a id="sec-3" name="sec-3"></a>

## Building Description

<a id="sec-3-1" name="sec-3-1"></a>

### usr_prj_cfg File Configuration Description

The file is used to configure user-defined information of a user project.

- The **usr_prj_cfg** file is stored in the `$WORK_DIR/hardware/vivado_design/example/example1/prj/` directory. (All subsequent operations are performed in this directory.)

To edit the **usr_prj_cfg** file, run the following commands:

```bash
  $ cd $WORK_DIR/hardware/vivado_design/example/example1/prj/
  $ vim ./usr_prj_cfg
```

The content of `usr_prj_cfg` is listed in the following figure.

| Content                                  | Keyword           | Example                                  |
| :--------------------------------------- | :---------------- | :--------------------------------------- |
| Name of the project created by the user  | USR_PRJ_NAME      | USR_PRJ_NAME=ul_pr_top                   |
| Top-layer name of the project            | USR_TOP           | USR_TOP=ul_pr_top                        |
| Synthesis policy designated by the user  | USR_SYN_STRATEGY  | USR_SYN_STRATEGY=AreaOptimized_high      |
| Implementation policy designated by the user | USR_IMPL_STRATEGY | USR_IMPL_STRATEGY=Explore                |
| User-defined constraints                 | USR_CONSTRAINTS   | USR_CONSTRAINTS="set_multicycle_path -setup -from [get_pins cpu_data_out*/D] 3" |

---

Configure **usr_prj_cfg** of the user-defined project according to the `example` in the preceding table:

- USR_PRJ_NAME=`Name of the project created by the user`, for example, **ul_pr_top** or **usr_prjxx_top**.
- USR_TOP=`Top-Layer name of the project`, for example, **ul_pr_top** or **usr_prjxx_top**.
- USR_SYN_STRATEGY=`Synthesis policy`, for example, **AreaOptimized_high**. It is user-defined. The default value is **DEFAULT**.
- USR_IMPL_STRATEGY=`implementation policy`, for example, **Explore**. It is user-defined. The default value is **DEFAULT**.
- USR_CONSTRAINTS=`user-defined constraints`. Generally, it is in the default state and does not need to be changed.

<a id="sec-3-2" name="sec-3-2"></a>

### build.sh Script Operation Instructions

The `build.sh` script is used to build a project. The script supports both one-click building and building step by step. For details, run the `-h` command. The command is as follows:

```bash
  $ sh build.sh -h
```

| Parameter                  | Description                              |
| :------------------------- | :--------------------------------------- |
| [-s] or [-S] or [-synth]   | Run the synthesis policy in a single step. |
| [-i] or [-I] or [-impl]    | Run the implementation policy in a single step. |
| [-p] or [-P] or [-pr]      | Run the PR verification in a single step. |
| [-b] or [-B] or [-bit]     | Run the target file generation in a single step. |
| [-e] or [-E] or [-encrypt] | No encryption for the synthesis policy.  |
| [-t num]                   | Build after [num] seconds                |
| [-h] or [-H] or [-help]    | Help for build.sh                        |
| [-s_strategy_help]         | Help for synthesis policy                |
| [-i_strategy_help]         | Help for implementation policy           |

To use one-click project building, run the following command:

```bash
  $ sh build.sh
```

  This command is used to complete the **synthesis policy** and **placing and routing** in one-click mode. The whole project runs PASS only if `all steps are implemented successfully`.  
  Note: **PR verification** and **bit file generation** can also be implemented in a single step. For details, see single-step execution description in [build.sh Script Operation Instructions](#sec-3-2).  

---

`build.sh` can also be used to implement a compilation task in a single step.

- Run the **synthesis** command in a single step:

```bash
  $ sh build.sh -s
```

If `"synth_design completed successfully."` is displayed when you run the synthesis printing in a single step, it indicates that the synthesis is successful.

---

- Run the **implementation** command in a single step:

```bash
  $ sh build.sh -i
```

If `"route_design completed successfully"` is displayed when you run the placing and routing in a single step, the placing and routing are successful.

---

- Run the **PR verification** command in a single step:

```bash
  $ sh build.sh -pr
```

If `"PR_VERIFY: check points /huaweicloud-fpga/.../usr_prjxx/prj/build/checkpoints/to_facs/usr_prjxx_routed.dcp and /huaweicloud-fpga/.../lib/checkpoints/SH_UL_BB_routed.dcp are compatible"` is displayed when you run the PR verification printing in a single step, the PR verification is successful.

---

- Run the **bit file generation** command in a single step:

```bash
  $ sh build.sh -b
```

If `"Bitgen Completed Successfully."` is displayed when you run the bit file generation in a single step, the bit file generation is generated successfully.

---

When users view the execution progress printing, they can read whether the execution result of each step in the Vivado tool is successful and whether the final result is PASS.

---

<a id="sec-3-3" name="sec-3-3"></a>

### schedule_task.sh Script Operation Instructions

The `schedule_task.sh` script is used to complete the scheduled building for the configurable time. For details, run the `-h` command as follows:

```bash
  $ sh schedule_task.sh -h
```

`schedule_task.sh` has four parameters: `day, hour, minute and second`.
In addition, `schedule_task.sh` supports two execution modes:

- Mode 1: Delay Execution

  Run the `schedule_task.sh *h` or `schedule_task.sh *m` command to execute the project compilation in certain hours or minutes. `*` indicates the value set by the user. You can also run `schedule_task.sh *h *m`, which indicates that the project is executed in certain hours and minutes.

  If the unit is not specified, the default unit is s (seconds). Other units include: m (minutes), h (hours), and d (days). For details, see the following example:

```bash
  $ sh ./schedule_task.sh 1m      # after 1 minute run
  $ sh ./schedule_task.sh 1h      # after 1 hour run
  $ sh ./schedule_task.sh 1d      # after 1 day run
```

---

- Mode 2: scheduled execution

  Run the `schedule_task.sh hour:minute` command to build the project at the specified time.

   If the time is not specified, the task is executed immediately. For details, see the following example:

```bash
  $ sh ./schedule_task.sh 11:50   # run project at 11:50
  $ sh ./schedule_task.sh 23:00   # run project at 23:00
```

<a id="sec-3-5" name="sec-3-5"></a>

### AEI_Register.sh Script Operation Instructions

This command is used to verify the PR, generate and upload the .bit file to the OBS bucket, and obtain the registration ID.
The format of the command for running the **AEI_Register.sh** script is as follows:

```bash
  $ sh AEI_Register.sh -n [AEI_name] -d [AEI_Description]

  # -n specifies the AEI name of the FPGA image to be registered.
  # AEI_name is a string of 1 to 64 characters consisting of uppercase and lowercase letters, digits, underscores (_), and hyphens (-).
  # -d specifies the AEI description of the FPGA image to be registered.
  # AEI_Description consists of 0 to 255 characters, including uppercase and lowercase letters, digits, hyphens (-), underscores (_), periods (.), commas (,), and spaces..
```

**Notes**

- Running the **AEI_Register.sh** script requires completing the `PR verification, ` `bit file generation`, and `registration ID generation`. It takes some time to finish these three steps.

- The **AEI_Register.sh** script is executed successfully if the following output is displayed:

```bash
  #############################################################
  Register AEI
  #############################################################
  Uploading FPGA image to OBS
  Upload 46696040 bytes using 2.00751 seconds
  Registering FPGA image to FIS
  Success: 200 OK
  id: 0000********5568015e3c87835c0326
  status: saving
```

- `Success: 200 OK` indicates that the **AEI_Register.sh** script is executed successfully and the .bit file is uploaded to the OBS bucket.
- `status: saving` indicates that the binary file registered by the user is in the saving state.

<a id="sec-4" name="sec-4"></a>

## Others

After the building is complete, files such as `vivado.log` and `ul_pr_top_terminal_run.log` are generated. If the building fails, users can locate the cause of the failure based on the logs.

