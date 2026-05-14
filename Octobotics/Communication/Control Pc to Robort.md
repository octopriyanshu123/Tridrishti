@Sanjeev and Tanmay
@receave 
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
3. Load setting    req json/struct Config{
     
     }
     req bool Status
4. Toggle Acquisition
	   req bool setAcquisitionStatus
	   res bool currentAcquisitionStatus 

@Send
1. Ascan 
   stream of Ascan 
   struct ascanData {
   int int Matrix 
   Depth x Amplitute Graph
   }