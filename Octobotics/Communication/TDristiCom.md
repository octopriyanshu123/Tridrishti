1 Connection

Control PC 
1 Establish the TCP connection 3 way Hand Shake
Session started 

---
----
---
---
Session End
2 Distroy the TCP connection 4 way Hand Shake
1 Jump to Step 1

2 Service
Blocking Call
Sync
req ->
res <--
ex Button Feature

3 Action
UnBlocking Call
Async
req Task ->
res Accept <-
stream feedback <-
res Task Commpletion <-

4 Data Streram
Contionus strem 
ex 
joystick Control Pc to Bot 
Ascan Bot to Control Pc

Github Referance
https://github.com/cpp-main/cpp-tbox
https://github.com/robotology/yarp
https://github.com/robosoft-ai/SMACC