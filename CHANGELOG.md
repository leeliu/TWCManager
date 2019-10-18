# ChangeLog

This document shows the changes per release

## v1.1.4 - Current Dev Branch

  * TODO: Some doco around module configuration
  * TODO: DSMR from https://github.com/dschutterop/dsmr
  * Implemented Tesla login / token retrieval for HTTP Config module.
  * Improved debug logging for Tesla API Vehicle module.
  * Breaking Change: Improved Settings Storage for non-volatile settings (not configuration) storage. 
    * This will require a manual port of the settings from ```/etc/twcmanager/TWCManager.settings``` to ```/etc/twcmanager/settings.json```
    * Due to a small user base, this is not done automatically. If there is demand for a port method, an issue can be raised for a feature request, and I'll create a one time script to port old settings to new settings.

## v1.1.3 - 2019-10-17

  * Added new HTTP Control Module for (limited) in-built HTTP control of TWCManager.
  * Added new Tesla Powerwall 2 EMS module for solar/consumption tracking.
  * Separation of Slave TWC code from Master TWC code - this adds stability by r
emoving a large number of global variables and reduces complexity for future fea
ture improvements.
     * Please Note: This is a major structural change to TWCManager, and is expected to take some time to fully test and validate. A benefit of this change is that now TWCSlave is modular, we can fully implement modular control interfaces.
     * Tested: TM as Master, tracking Green Energy
     * Untested: TM as Slave (low-pri)
        * Will not work. There's some slave related comms that need to move from TWCManager to TWCMaster
     * Tested: TM as Master, Scheduled Charging
     * Untested: TM as Master, Charge Now
     * Untested: Web IPC interface
     * TODO: Clean up globals in TWCSlave
       * All that is left is the TM as Slave change above
     * TODO: Look at globals in TWCManager
     * TODO: Fix bug - need to subtract charger load from consumption in regular status message IF option is enabled

## v1.1.2 - 2019-10-14

  * Improvements to HomeAssistant EMS module to avoid setting Consumption/Generation values to zero as a result of a connection failure, and better exception handling in general for the module.
  * Added EMS support for TED (The Energy Detective)
  * Added Control module support with a first control module for MQTT, with initial commands to stop the TWCManager daemon and start charging immediately.
  * Improvements to the MQTT and HASS topic/sensor names for Slave TWCs. (4 byte instead of 2 byte)

## v1.1.0 - 2019-10-12

  * Separation of Tesla API class from TWCManager code, this allows pluggable modules for interfacing with vehicles.
  * Added Fronius EMS module - read generation and consumption values from a Fronius inverter.
  * Added MQTT authentication for MQTT Status module.
  
### Fixes

  * Fixed settings file location path which had issues if no trailing slash.

## v1.0.2 - 2019-10-11

  * Option to subtract charger utilization from consumption value if consumption meter is counting charger load.
  * Updated status output to more clearly define the generation and consumption values being tracked.

## v1.0.1 - 2019-10-06

This release is a bugfix release for v1.0.0. It contains the following fixes:

  * Fixed: MQTT status connection timeout causes a delay to heartbeat timers, which then cause TWCManager reconnections to Slaves
  * Fixed: MQTT and HASS status update rates are too high. All updates now rate limited to one per minute.

## v1.0.0 - 2019-10-05

This module provides HomeAssistant sensor functionality to the TWCManager project. It is the first release after the fork and restructuring of the code and configuration files.

  * Forked original repository by cdragon
  * Integrated multi-charger and multi-car MQTT integration (Status Module) from flodorn's repository
  * Split the Sensor (EMS) and Status functions out of the main script
  * Created HomeAssistant EMS and Status modules and tested successfully
  
### Known Issues

  * Large numbers of uncaught exceptions around connectivity for EMS and Status interfaces - These can be safely ignored, they will be fixed in v1.1. Currently, they will only result in excessive logging.
  
### Future Changes

  * Separate class for serial communications - so TWCMaster and TWCSlave don't both need to speak to the serial port directly
  * Modular serial interface - allow dummy module to be substituted for testing (along with above work)