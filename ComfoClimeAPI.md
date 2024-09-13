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
| /device/$DEVID$/definition | reads some basic data for the device | |

# 
