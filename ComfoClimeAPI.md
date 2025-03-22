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
| /system/$UUID$/thermalprofile | reading/setting thermal profile | some values |
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

## Available units
| DEC  | HEX  | subunit count | name | usage |
|------|------|---------------|------|-------|
| 1    | 0x01 | 1             | NODE | General node information |
| 2    | 0x02 | 1             | ??   | ?? |
| 3    | 0x03 | 1             | ??   | ?? |
| 21   | 0x15 | 2             | ??   | ?? |
| 22   | 0x16 | 1             | TEMPCONFIG | Configuration data (season, temp values, ...) |
| 23   | 0x17 | 1             | HEATPUMP | Configuration of heatpump |
| 25   | 0x19 | 1             | ??   | ?? |
| 26   | 0x1A | 1             | ??   | ?? |

## Properties of subunits

| Unit | SubUnit | Property | Access | Format | Description |
|------|---------|----------|--------|--------|-------------|
| NODE | 1       | 1        |        | UINT8  | ?? = 1      |
| NODE | 1       | 2        |        | UINT8  | ?? = 20      |
| NODE | 1       | 3        |        | UINT8  | ?? = 1      |
| NODE | 1       | 4        |        | STRING | Serial number MBE...  |
| NODE | 1       | 5        |        | UINT8  | ?? = 1      |
| NODE | 1       | 6        |        | UINT8  | Possibly firmware version |
| NODE | 1       | 7        |        | UINT32 | ?? = [0,120,16,192] |
| 2    | 1       | 1        |        | UINT8  | ?? = 1      |
| 2    | 1       | 2        |        | UINT8  | ?? = 1, 0 |
| 2    | 1       | 3        |        | UINT8  | ?? = 0      |
| 2    | 1       | 4        |        | UINT8  | ?? = 0      |
| 3    | 1       | 1        |        | UINT8  | ?? = 1      |
| 21   | 1       | 1        |        | UINT8  | ?? = 1      |
| 21   | 2       | 1        |        | UINT8  | ?? = 1      |
| 22   | 1       | 1        |        | UINT8  | ?? = 1      |
| 22   | 1       | 2        |        | UINT8  | season automatic on/off |
| 22   | 1       | 3        |        | UINT8  | season select (0 = transitional, 1 = heating, 2 = cooling) |
| 22   | 1       | 4        |        | UINT16 | curve kneepoint heating |
| 22   | 1       | 5        |        | UINT16 | curve kneepoint cooling |
| 22   | 1       | 8        |        | UINT8  | automatic temperature select |
| 22   | 1       | 9        |        | UINT16 | comfort temp heating |
| 22   | 1       | 10       |        | UINT16 | comfort temp cooling |
| 22   | 1       | 11       |        | UINT16 | maximum temp while cooling |
| 22   | 1       | 12       |        | UINT16 | heating delta/difference |
| 22   | 1       | 13       |        | UINT16 | manual mode target temperature |
| 22   | 1       | 15       |        | UINT16 | minimum outdoor temp for cooling |
| 22   | 1       | 16       |        | UINT16 | maximum outdoor temp for heating |
| 22   | 1       | 17       |        | UINT16 | ?? = [91,1],[94,1] |
| 22   | 1       | 18       |        | UINT8  | ?? = 70     |
| 22   | 1       | 19       |        | UINT16 | ?? = [232,3] |
| 22   | 1       | 20       |        | UINT16 | ?? = [50,0] |
| 22   | 1       | 21       |        | UINT8  | ?? = 0 |
| 22   | 1       | 23       |        | UINT8  | ?? = 10 |
| 22   | 1       | 24       |        | UINT16 | ?? = [3,0] |
| 22   | 1       | 25       |        | UINT8  | ?? = 0 |
| 22   | 1       | 26       |        | UINT8  | ?? = 1 |
| 22   | 1       | 27       |        | UINT8  | ?? = 0 |
| 22   | 1       | 28       |        | UINT8  | ?? = 0 |
| 22   | 1       | 29       |        | UINT8  | ComfoClime mode (0 = Comfort, 1 = Power, 2 = Eco) |
| 22   | 1       | 40       |        | UINT8  | ?? = 0 |
| 22   | 1       | 41       |        | UINT16 | ?? = [0,0] |
| 22   | 1       | 42       |        | UINT8  | ?? = 0 |
| 23   | 1       | 2        |        | UINT16 | ?? temp = 30,0°C |
| 23   | 1       | 3        |        | UINT16 | heating pump max temp |
| 23   | 1       | 4        |        | UINT16 | heating pump min temp |
| 23   | 1       | 5        |        | UINT16 | ?? temp = 20,0°C |
| 23   | 1       | 6        |        | UINT16 | ?? temp = 5,0°C |
| 23   | 1       | 7        |        | UINT16 | ?? temp = 60,0°C |
| 23   | 1       | 8        |        | UINT16 | ?? temp = 0,0°C |
| 23   | 1       | 9        |        | UINT16 | ?? temp = 0,1°C |
| 23   | 1       | 10       |        | UINT16 | ?? temp = 1,5°C |
| 23   | 1       | 11       |        | UINT8  | ?? = 0 |
| 23   | 1       | 12       |        | UINT16 | ?? = [0,0],[1,0] |
| 23   | 1       | 13       |        | UINT16 | ?? = [0,0] |
| 23   | 1       | 14       |        | UINT16 | ?? = [200,0],[214,1] |
| 23   | 1       | 15       |        | UINT8  | ?? = 100 |
| 23   | 1       | 16       |        | UINT8  | ?? = 100 |
| 23   | 1       | 17       |        | UINT16 | ?? = [25,0] |
| 23   | 1       | 18       |        | UINT16 | ?? = [45,0] |
| 23   | 1       | 19       |        | UINT16 | ?? = [2,0] |
| 23   | 1       | 32       |        | UINT32 | ?? = [0,0,0,0] |
| 25   | 1       | 1        |        | UINT8  | ?? = [2,0]      |
| 26   | 1       | 1        |        | UINT8  | ?? = 0     |


# ComfoClime Sensors

| Telemetry number | Format | Description |
|------------------|--------|-------------|
| 1-784 | | N/A |
| 785 | | = 0 ?? |
| 3500-4111 | | N/A |
| 4112 | | = 0 ?? |
| 4113 | | = 0 ?? |
| 4116 | | = 0 ?? |
| 4117 | | = 255 ?? |
| 4120 | | = 0 ?? |
| 4121 | | = 0 ?? |
| 4124 | | = [255,255,255,255] ?? |
| 4125 | | = [0,0,0,0] ?? |
| 4128 | | = [1,0] ?? |
| 4129 | | = [0,0] ?? |
| 4132 | | = [0,0] ?? |
| 4133 | | = [0,0] ?? |
| 4145 | | TPMA temperature |
| 4146 | | time ?? |
| 4148 | | target temperature? |
| 4149 | | ComfoClime mode (off=0, heating=1, cooling=2) |
| 4150 | | = 0 ?? |
| 4151 | | current heating comfort temperature |
| 4152 | | comfort temp minus difference |
| 4153 | | temperature ?? [3,0] |
| 4154 | | indoor temperature |
| 4193 | | supply temperature |
| 4194 | | exhaust temperature ?? |
| 4195 | | heat pump supply side temperature ?? |
| 4196 | | exhaust temperature ?? |
| 4197 | | heat pump exhaust side temperature ?? |
| 4198 | | = 0 ?? |
| 4199 | | = 0 ?? |
| 4201 | | current power |
| 4202 | | = [52,1] ?? |
| 4203 | | = [0,0] ?? |
| 4204 | | = [0,0] ?? |
| 4205 | | = [81,0] ?? |
| 4206 | | = [5,0] ?? |
| 4207 | | = [2,0] ?? |
| 4208 | | = [200,0] ?? |
| 4209 | | = 0 ?? |
| 4210 | | = 0 ?? |
| 4211 | | = [4,0] ?? |
| 4272 | | = [0,0,0,0] ?? |
| 4273 | | = [0,0,0,0,0,0,0,0] ?? |
| 4305 | | = 0 ?? |
| 4306 | | = [0,0,0,0] ?? |



