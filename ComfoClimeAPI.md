# IDs

The interface has a UUID, which can be polled with the first two API requests. All "/system/..." requests will then need the UUID of this interface.
Each device connected to the ComfoNet will also have a device ID (DEVID). This can be used by querying data from the individual device. Some device don't have a device ID (for example the option box). DEVID is NULL in these cases.


# Known API Endpoints 

| API endpoint | function | additional information |
|--------------|----------|------------------------|
| /monitoring/ping | UUID of interface, uptime, timestamp | retreived on first connect |
| /system/systems | location and connection of ComfoClime | |
| /system/$UUID$/devices | list of all connected devices | on ComfoNet Bus / CAN Bus |
| /system/$UUID$/dashboard | data for dashboard in app | temperatures, fanspeeds, ... |
| /system/$UUID$/alarms | all errors of connected devices | seems to contain history |
| /device/$DEVID$/property/X/Y/Z | reading properties of device | |
| /device/$DEVID$/telemetry/N | reading sensor values from device | |
| /device/$DEVID$/definition | reads some basic data for the device | |

# Reading properties

The ./property endpoint reads values from the ComfoNet bus similiar to the RMI Protocol (as described in https://github.com/michaelarnauts/aiocomfoconnect/blob/master/docs/PROTOCOL-RMI.md). 
The address translates like:
/device/$DEVID$/property/X/Y/Z
X = Unit
Y = Subunit
Z = Property
For example querying /device/.../property/1/1/4 on a ComfoAir device returns the serial number of the device. You always receive data as byte array in decimal representation.
Changing a value in the app will result in a PUT request to the property. 

The ./telemetry endpoint reads sensor values from the ComfoNet bus similiar to the PDO protocol (as described in https://github.com/michaelarnauts/aiocomfoconnect/blob/master/docs/PROTOCOL-PDO.md)., where "N" is the number of the sensor.


# ComfoClime Nodes

| Unit | SubUnit | Property | Access | Format | Description |
|------|---------|----------|--------|--------|-------------|
| NODE | 1       | 1        |        | UINT8  | ?? = 1      |
| NODE | 1       | 2        |        | UINT8  | ?? = 20      |
| NODE | 1       | 3        |        | UINT8  | ?? = 1      |
| NODE | 1       | 4        |        | STRING | Serial number MBE...  |
| NODE | 1       | 5        |        | UINT8  | ?? = 1      |
| NODE | 1       | 6        |        | UINT8  | Possibly firmware version |
| NODE | 1       | 7        |        | UINT32 | ?? = [0,120,16,192] |

# ComfoClime Sensors

| Telemetry number | Format | Description |
|------------------|--------|-------------|
| 4145 | | TPMA temperature |
| 4146 | | time ?? |
| 4148 | | temperature ?? |
| 4149 | | ComfoClime mode ?? (0=Comfort/Eco,2=Power) |
| 4150 | | = 0 ?? |
| 4151 | | temperature ?? |
| 4152 | | temperature ?? |
| 4153 | | temperature ?? [3,0] |
| 4154 | | indoor temperature |


