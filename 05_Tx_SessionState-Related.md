# Tx Session State-Related


DexAppKit defines a number of objects for which the app cores (G7AppCore/D1AppCore) can convert their transmitter model specific versions into more generic ones, to save the client from needing to handle two different codepaths.


## DisplayType (UInt8 Enum)

Type                 | Value  | Notes
-------------------- | ------ | ------------------
| Unknown	           | 0	    | Default Value
| medical            | 1      |
| phone              | 2      |
| watch              | 3      |
| receiver           | 4      |
| pump               | 5      |
| reader             | 6      |
| tool               | 7      |
| other              | 8      |
| transmitter        | 9      | Used internally by FW team

## DeviceList

Should not be used to indicate watch support or capability.

Property/Method               | Type                  | Description
----------------------------- | --------------------- | ------------------
| connectedDisplays           | List of DisplayType   | Unknown type should be filtered out
| isConnected(DisplayType)    | Bool                  | Check whether the given DisplayType is currently connected


## TransmitterInfo

Property             | Type              | Description
-------------------- | ----------------- | ------------------
| softwareNumber     | Int               | The SW number for the transmitter
| firmwareVersion    | String	           | The full version number for the connected transmitter
| activateBy         | CalculatedTime?   | The date which this transmitter should be started by (G6/D1 only)


## LastCalibration

This contains only successful calibrations. This information comes from the calibration bounds response.

Property       | Type             | Description
-------------- | ---------------- | ------------------
| meterValue	 | Mgdl             | The submitted meter value
| processTime  | CalculatedTime   | The time the transmitter processed the calibration


## UserCommandType (enum)

Name      |                 
----------|
stop      |
calibrate |
start     |


## ManualStopReason (enum)

These all map to stop reasons for a G7, but could later support other future systems as well.

Type            |      
----------------|
none            |
skip            |
other           |
bestTimingForMe |
inaccurate      |
noReadings      |
sensorFellOff   |
discomfort      |


## PendingUserCommand


Property       | Type              | Description
-------------- | ----------------- | ------------------
| type         | UserCommandType   | The type of command that is pending
| queuedTime   | CalculatedTime    | Time at which the command was queued
| meterValure  | Mgdl              | Only used by calibration type
| stopReason   | ManualStopReason  | Only used by stop type
| SensorCode   | String            | Only used by start type


Methods on a PendingUserCommand Array/List


Method                       | Description                
-----------------------------| ---------------------
isStopPending → Bool         | Is one of the commands in the list a stop
isCalibrationPending → Bool  | Is one of the commands in the list a calibration
isStartPending → Bool	       | Is one of the commands in the list a start

## UserCommandResult


Property         | Type                  | Description
---------------- | --------------------- | ------------------
| type	         | UserCommandType       | The type of command that was executed
| accepted       | Bool                  | Simple yes/no whether the command was accepted
| processTime    | CalculatedTime        | Time at which command was processed by transmitter


## TrendArrow (Enum)


Type                | Value Range               
--------------------| ---------------------
snooze_enabled      | Anything that doesn't fall into the below ranges
doubleUp            | 3 <= x <= 8
| singleUp          | 2 <= x < 3
| fortyFiveUp       | 1 <= x < 2
| flat              | -1 < x < 1
| fortyFiveDown     | -2 < x <= -1
| singleDown        | -3 < x <= -2
| doubleDown        | -8 <= x <= -3


## ShowEgv (Enum)

This tells the client whether to show the actual glucose value or to indicated simply 'High' or 'Low'.


Type     | Description                
---------| ---------------------
value    | 	Show the actual glucose value
low      | Indicate the value is simply 'Low'
high     | Indicate the value is simply 'High'

Type                         | Notes       | Description
---------------------------- | ----------- | ------------------
| displayTypeRestricted      |             | This means another display of the same type is already connected. The app should show a screen or message saying the user needs to stop using their other phone/watch
| wrongPairingCode           | G7 Only     | This means we found another device but it had a different pairing code than we were looking for. The app should show a message asking them if they entered the correct pairing code.
| transmitterLostBond        | iOS Only    | The transmitter lost the bond information, so now the display is refusing to connect to the transmitter. The user needs to manually remove the bond information under Bluetooth Settings
| tooManyDevicesConnected    | iOS Only    | The OS is reporting that too many devices are already connected. The user needs to remove some Bluetooth devices
