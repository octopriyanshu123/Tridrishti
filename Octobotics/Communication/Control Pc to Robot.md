### Tanmay -> Sanjeev 
1. Connect 
	req struct {
	string ip
	string port
	string deviceType
	}
	res bool status
2. Disconnect 
	req struct {
	string ip
	string port
	string **deviceType**
	} 
	res bool status
3. Load setting    
    req json/struct Config{
     
     }
     req bool Status
4. Toggle Acquisition
	   req bool setAcquisitionStatus
	   res bool currentAcquisitionStatus 

### Sanjeev <- Tanmay
1. Ascan 
   stream of Ascan 
   struct ascanData {
   int int Matrix 
   Depth x Amplitute Graph
   }

## Tanmay -> Robot

| S.no | Name                                            | Type      | Request                             | Feedback                 | Responce                       | Description                          |
| ---- | ----------------------------------------------- | --------- | ----------------------------------- | ------------------------ | ------------------------------ | ------------------------------------ |
| 1    | Joystick Event                                  | Stream    |                                     |                          | Struct<br>[[JoystickMsg.hpp]]  |                                      |
| 2    | Motor On/off                                    | AsyncTask | Motor id (int)                      |                          |                                | Task to Power up the motor           |
| 3    | Stepper Motor On/off                            | AsyncTask | void                                |                          |                                | Task to Power up the Stepper motor   |
| 4    | Linear Actuator On/off                          | AsyncTask | void                                |                          |                                | Task to Power up the Linear Actuator |
| 5    | Calibration                                     | Action    | void                                | Percentage of completion | Completion status<br>(bool)    | Calibration All sensor               |
| 6    | Set Inspection Length<br>(BtMetaData )          | Service   | Inspection Length<br>(int)          |                          | SetService<br>status<br>(bool) |                                      |
| 7    | Set Raster Inspection Speed<br>(BtMetaData )    | Service   | Inspection Speed<br>(int)           |                          | SetService<br>status<br>(bool) |                                      |
| 8    | Set Linear Inspection Distance<br>(BtMetaData ) | Service   | Linear Inspection Distance<br>(int) |                          | SetService<br>status<br>(bool) |                                      |
|      |                                                 |           |                                     |                          |                                |                                      |
## Tanmay <- Robot

| SNo | Name                      | Type   | Request | Feedback | Responce                               | Description |
| --- | ------------------------- | ------ | ------- | -------- | -------------------------------------- | ----------- |
| 1   | Status of Motor           | Stream |         |          | Struct<br>[[MotorStatus.hpp]]          |             |
| 2   | Status of Linear Actuator | Stream |         |          | Struct<br>[[LinearActuatorStatus.hpp]] |             |
|     |                           |        |         |          |                                        |             |
