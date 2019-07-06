# HVAC IR Control


## Introduction
*HVAC IR Control aims to facilitate control of your HVAC emulating the IR code from an Arduino (a python port for the RPI is also available). This Do It Your Self has no limitation except the time to spent on it. I hope this repository can accelerate your development espcecially if you use a Mitsubishi HVAC. Panasonic HVAC support has been added too, thanks to another contributor. Should you want to add another protocol data information releated to new brand or model then feel free to contact us*

## Project background
I started to use sketch with an Arduino associated to an IR emitter and IR Receiver. I quickly learned that the libraries available do not cover HVAC modules. One way to progress was to use a software named AnalysIR, therefore i ordered a license for this tool. Indeed this tool was perfect for the job of identifying the IR frames my IR remote was sending. Nevertheless, even if the data collected was able to identify bits values, the packet of data provided to use with the different existing libraries was a set of mark and space interger values. For an Arduino it's a lot of data only for one command to process in IR. Due to this limitation of memory, I started to think about coding a function using the hex values of the frame decoded by AnalysisIR instead of having to manage mark and space using a huge array of data. Without AnalysisIR software, it never would have been possible for me to achieve what i did. Thanks Chris ;).

My first code was able to take input of hex data for the decoded frame and setting the diffrent header pulse period, I went through a loop of each bit in order to produce the correct mark and space sequences. I identified quickly that the packet of data was in fact a specific packet of data sent twice. After this dicovery, I started to look at the values of the data from this packet. I was entering into reverse engineering of the Mitsubishi frame. In order to understand the protocol, I started to log different configurations and the packet data associated. I've used an Excel file to start to dig into the differents bits changing folloing the configurations. Hopefully the CRC was not complex. I left this excel debug file in the repository. It might be helpful for others.

Finally the packet data has been decoded at least for my HVAC system. The protocol decoded, I decided to use IR-Remote to add specific functions to control a Mitsubishi HVAC. Simply pass the parameters and the IR frame is compiled and send to the HVAC. No more problem of memory. A colleague joined this work having on his side a PANASONIC HVAC. The same methods have been applied to understand the Panansonic protocol. Like for the Mitsubishi HVAC, the Panasonic HVAC functions have been added to this git repo, still based on a modified IR Remote.

Recently, an anonymous and humble contributor provided information on unknown parts of the Mitsubishi protocol. All new fields decoded have been added to the Protocol Information data. The code has not been modified yet. 

# Overview of Protocol and features managed

## Mitsubishi Inverter HVAC

There are two kind of functions you may use to control an HVAC from Mitsubishi. Thanks to an anonymous contributor, I have the opportunity to complete the documentation for this protocol. Basically he got many more options than I have on my HVAC. Nevertheless, just a warning, I did not have the possibility to check all these new features, so do not hesitate to report issue. Enjoy!

The function to send configuration is:

```
void sendHvacMitsubishi(
 HvacMode                  HVAC_Mode,           // Example HVAC_HOT  HvacMitsubishiMode
 int                       HVAC_Temp,           // Example 21  (°c)
 HvacFanMode               HVAC_FanMode,        // Example FAN_SPEED_AUTO  HvacMitsubishiFanMode
 HvacVanneMode             HVAC_VanneMode,      // Example VANNE_AUTO_MOVE  HvacMitsubishiVanneMode
 int                       OFF                  // Example false (Request Turn On = False)
);
```
new function with enhanced function:
```
void sendHvacMitsubishiFD(
 HvacMode                  HVAC_Mode,           // Example HVAC_HOT  HvacMitsubishiMode
 int                       HVAC_Temp,           // Example 21  (°c)
 HvacFanMode               HVAC_FanMode,        // Example FAN_SPEED_AUTO  HvacMitsubishiFanMode
 HvacVanneMode             HVAC_VanneMode,      // Example VANNE_AUTO_MOVE  HvacMitsubishiVanneMode
 HvacAreaMode              HVAC_AreaMode,       // Example AREA_AUTO
 HvacWideVanneMode         HVAC_WideMode,       // Example WIDE_MIDDLE
 int                       HVAC_PLASMA,          // Example true to Turn ON PLASMA Function
 int                       HVAC_CLEAN_MODE,      // Example false 
 int                       HVAC_ISEE,            // Example False
 int                       OFF                   // Example false to Turn ON HVAC / true to request to turn off
 );
```

Functions confirmed in MSZ-GE and MFZ modules from Mitsubishi.

<table>
    <tbody>
        <tr>
            <td align="center">
            <a href="https://github.com/Ericmas001" target="_blank">
            Ericmas001 <br />
            <img src="https://avatars1.githubusercontent.com/u/11448087?v=3&s=80" alt="Ericmas001" width=50 />
            </a>
            </td>
            <td align="left">
                <div class="nuget-badge">
                    <b>A python port has been made to be compatible with Raspberry Pi.</b> <br />
                    <code>pip install git+https://github.com/Ericmas001/HVAC-IR-Control</code> <br />
                    Big thanks to <a href="https://github.com/r45635" target="_blank">r45635</a>, <a href="https://github.com/bschwind" target="_blank">bschwind</a>, <a href="https://github.com/r45635" target="_blank">danijelt</a> who made this possible to achieve !
                </div>
            </td>
        </tr>
    </tbody>
</table>

## Panasonic HVAC

the function to send configuration is

```
void sendHvacPanasonic(
 HvacMode                  HVAC_Mode,           // Example HVAC_HOT  HvacPanasonicMode
 int                       HVAC_Temp,           // Example 21  (°c)
 HvacFanMode               HVAC_FanMode,        // Example FAN_SPEED_AUTO  HvacPanasonicFanMode
 HvacVanneMode             HVAC_VanneMode,      // Example VANNE_AUTO_MOVE  HvacPanasonicVanneMode
 HvacProfileMode           HVAC_ProfileMode,    // Example QUIET HvacProfileMode
 int                       HVAC_SWITCH          // Example false
);
```

## Toshiba HVAC

the function to send configuration is

```
void sendHvacToshiba(
 HvacMode                  HVAC_Mode,           // Example HVAC_HOT  
 int                       HVAC_Temp,           // Example 21  (°c)
 HvacFanMode               HVAC_FanMode,        // Example FAN_SPEED_AUTO  
 int                       HVAC_SWITCH          // Example false
);
```
