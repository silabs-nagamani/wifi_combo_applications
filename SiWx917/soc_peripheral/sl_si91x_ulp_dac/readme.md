# SL ULP DAC

## Table of Contents

- [Purpose/Scope](#purposescope)
- [Overview](#overview)
- [Prerequisites/Setup Requirements](#prerequisitessetup-requirements)
  - [Hardware Requirements](#hardware-requirements)
  - [Software Requirements](#software-requirements)
  - [Setup Diagram](#setup-diagram)
- [Getting Started](#getting-started)
- [Application Build Environment](#application-build-environment)
  - [Application Configuration Parameters](#application-configuration-parameters)
  - [Pin Configuration](#pin-configuration)
- [Test the Application](#test-the-application)

## Purpose/Scope

- This application demonstrate the DAC peripheral on ULP mode,
  - Conversion of digital input to 10-bit analog output.
  - Samples the data.
  - Convert data into analog output based on operation mode.

## Overview

- The DAC Controller works on a DAC with a resolution of 10bits at 5MHZ sampling frequency maximum.
- For the clock setup to the DAC controller, the sample application default values are recommended.
- There are three operating mode in AUX DAC controller:
  - Static Mode Operation: DAC will give out constant voltage output for a programmed DAC.
  - FIFO Mode Operation: DAC controller can be kept in FIFO mode to play continuously digital word on DAC. This mode can be used for playing Single tone waveform in DAC.
  - Reference voltage for ADC operation: DAC controller can be kept in a mode where DAC output will be used as reference voltage to ADC channel input.
- DAC will work only in single ended.
- Monotonic by design - based on input sample it will give equivalent output.

## Prerequisites/Setup Requirements

### Hardware Requirements

- Windows PC
- Silicon Labs Si917 Evaluation Kit [WPK(BRD4002)+ BRD4338A]

### Software Requirements

- Simplicity Studio
- Serial console Setup
  - The Serial Console setup instructions are provided below:
Refer [here](https://docs.silabs.com/wiseconnect/latest/wiseconnect-getting-started/getting-started-with-soc-mode#perform-console-output-and-input-for-brd4338-a).

### Setup Diagram

> ![Figure: Introduction](resources/readme/setupdiagram.png)

## Getting Started

Refer to the instructions [here](https://docs.silabs.com/wiseconnect/latest/wiseconnect-getting-started/) to:

- Install Studio and WiSeConnect 3 extension
- Connect your device to the computer
- Upgrade your connectivity firmware
- Create a Studio project

## Application Build Environment

### Application Configuration Parameters

- Configure UC from the slcp component.
- Open **sl_si91x_ulp_dac.slcp** project file select **software component** tab and search for **DAC** in search bar.
- Using configuration wizard one can configure different parameters. Below are the configuration screen, where user can select as per requirement.

  - **DAC Peripheral Configuration**

    - DAC operation mode: There are 3 modes, Static mode, FIFO mode and Reference voltage for ADC. By default it is in Static mode.
    - Sample rate: Sample rate can be configurable to DAC, sample rate unit is samples/second. The configuration range from 63sps to 5Msps.
    - FIFO Threshold: This is applicable only for FIFO mode. There is no impact even configure for other modes. The configuration range from 0 to 7.
    - ADC Channel: This is applicable only for Reference voltage for ADC mode. There is no impact even configure for other modes. The channel number can configure 0 to 15.

      ![Figure: Introduction](resources/uc_screen/sl_dac_uc_screen.png)

- After running the application see the output on logic analyzer in GPIO_30[P35].
- Apply the different voltages(1.8V to 3.6v) as reference voltage and input samples, based on this observe output in analyzer .
- The output data should match the input sample that was supplied. Due to DAC electrical specifications the highest output voltage tolerance is 0.85v and lowest output voltage tolerance is 0.15v.

### Pin Configuration

- Here mentioned pin numbers for BRD4338a (B0 2.0v boards), if user want to use different radio board, refer to board specific user guide.

  | DAC | GPIO's |
  | --- | --- |
  | DAC0 | ULP_GPIO_4 |
  | DAC1 | ULP_GPIO_15 |

- Because of the GPIO workarounds described above, the DAC output in BRD4338a is read internally, supplied to OPAMP1, and allows the user to see the OPAMP1 output on the GPIO_30[P35] pin.

## Test the Application

Refer to the instructions [here](https://docs.silabs.com/wiseconnect/latest/wiseconnect-getting-started/) to:

1. Compile and run the application.
2. By default DAC will be in Static mode. The output in this mode will be a constant voltage.
3. Make use of a logic analyzer to examine the output in GPIO_30[P35].

   - DAC Static mode:

     - Console Output:

       ![Figure: Introduction](resources/readme/dac_static_console_output.png)

     - Logic Analyzer Output:

       ![Figure: Introduction](resources/readme/dac_static_analyzer_output.png)

   - DAC FIFO mode:

     - Console Output:

       ![Figure: Introduction](resources/readme/dac_fifo_console_output.png)

     - Logic Analyzer Output:

       ![Figure: Introduction](resources/readme/dac_fifo_analyzer_output.png)

> **Note:**
>
>- In the sl_ulp_dac_example.c file, on #line26, locate the 'STATIC_MODE' macro. If the DAC operation mode is set to Static, make sure to set the value to '1' for this macro. If the FIFO mode is    selected, substitute '0' for this macro.
>- It is possible to set the 'STATIC_MODE' macro value to either '0' or '1' for the reference voltage for the ADC mode of the DAC.
>- Buffer length should be '1' for DAC static mode.
>
 Use following formula to find equivalent output analog voltage of DAC
>
> DAC_output = ((input sample/1024(2^10)) * Vref Voltage);
>
> > **Note:** If input sample value is '0x33A' and voltage reference is 2.8v, then DAC_output = ((0x33A/1024) * 2.8).

## Important Note for using this project with wiseconnect SDK

sl_si91x_ulp_dac application is being migrated to the Power Manager, this application will be available in the wiseconnect SDK from next release.
To use the application now, follow below steps:-
**Step 1:-**
Copy sl_si91x_ulp_dac folder into Gecko SDK Suite i.e. gecko-sdk/extension/wiseconnect3/examples/si91x_soc/peripheral/ folder.
**Step 2:-**
Add the example details in the gecko-sdk/extension/wiseconnect3/wifi_templates.xml like below
```
<descriptors name="sl_si91x_ulp_dac" label="Si91x - SL ULP DAC" description="Demonstrates the how to configure and use the ULP DAC peripheral on the the SiWx917 SoC device.">
    <properties key="namespace" value="template.uc"/>
    <properties key="keywords" value="universal\ configurator"/>
    <properties key="projectFilePaths" value="examples/si91x_soc/peripheral/sl_si91x_ulp_dac/sl_si91x_ulp_dac.slcp"/>
    <properties key="boardCompatibility" value="com.silabs.board.none brd4325a brd4325b brd4325c brd4325e brd4325f brd4325g brd4338a brd4343a brd4343b brd4339b brd4340b brd4342a"/>
    <properties key="partCompatibility" value="mcu.arm.efr32.mg21.* mcu.arm.efr32.mg21.*  mcu.arm.efr32.mg24.* .*si917.* .*si917.* .*siwg917m111mgtba.* .*siwg917m111mgtba.* .*siwg917m141xgtba.* .*siwg917m111mgtba.* .*siwg917m111mgtba.* .*siwg917m111mgtba.* .*siwg917y111mgab.* .*siwg917y111mgnb.*"/>
    <properties key="ideCompatibility" value="makefile-ide simplicity-ide visual-studio-code generic-template"/>
    <properties key="toolchainCompatibility" value="gcc"/>
    <properties key="quality" value="production"/>
    <properties key="category" value="Example|Wi-Fi"/>
    <properties key="stockConfigCompatibility" value="com.silabs.ss.framework.project.toolchain.core.default"/>
    <properties key="sdkAndProtocolTags" value=""/>
    <properties key="readmeFiles" value="examples/si91x_soc/peripheral/sl_si91x_ulp_dac/readme.md"/>
	<properties key="filters" value="Device\ Type|SoC Project\ Difficulty|Basic"/>
  </descriptors>
```
**Step 3:-**
Update the Gecko-SDK version and wiseconnect SDK version in the sl_si91x_ulp_adc.slcp
In the Simplicity Studio click on prefernces -> search for sdk -> ![Know the SDK version and wiseconnect Version from Simplicity Studio](resources/uc_screen/dac_know_sdk_version_slcp.png)
Update the GSDK and wiseconnect SDK in ![slcp](resources/uc_screen/dac_change_version_slcp.png)
**Step 4:-**
Refresh the SDK in the Simplicity Studio by clicking on the preferences refer ![Simplicity Studio refresh SDK](resources/uc_screen/dac_refresh_sdk.png)
**Step 5:-**
Refer to the instructions [here](https://docs.silabs.com/wiseconnect/latest/wiseconnect-getting-started/) for getting started with example.