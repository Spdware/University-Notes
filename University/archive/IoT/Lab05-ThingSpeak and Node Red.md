We use the cloud as everyday the need of IoT solutions increases as having so many devices around collecting data we can enhance the collection of data as the devices are resource/power constraint. 
The term “platform” can refer to:
- Hardware architectures (ARM, Arduino, ESP32, etc...)
- Software frameworks to program smart things
- Cloud-based middleware platforms to manage IoT data and devices
We are focusing on the SW platform and not on the HW of the devices as the SW is the one that will process the great amount of data.
The features of an IoT platform should have:
- device management: access device information, update the device
- data storage: take the data from the devices and stores somewhere and accessible 24h
- data analysis and automation on top of the data
- security
# ThingSpeak
[https://thingspeak.com/]
Needs a MathWorks account. 
 An IoT cloud platform for:
- Real-time data collection and storage
 - Data analytics and visualization (integrated with MATLAB)
- Alerts and Scheduling
-  Device Communication, Open API
At the start you can specify all the parameters needed for the application. Then once created it is initially private but you can make a project public and accessible by their IDs.
ThingSpeak accept the publication of data through HTTP or MQTT.  There is an API for Writing and one for Reading. Data are stored in fields and you can append all the data you want.
When I use the API we have a feedback of how many updates goes correctly. You can concatenate up to 8 fields(maximum channels in a ThingSpeak channel). Pay attention that there is a rate limitation for the free version(20-30 seconds).
To allow MQTT interaction we need to create an authenticated device through the MQTT tab.

See MQTT ThingSpeak for credential
## Channel Field update: HTTP
cURL: CLI tool for transferring data using HTTP
Remember to escape “special” characters
curl https://api.thingspeak.com/update\?write_api_key\=YOUR_API_KEY\&field1\=XXXX
## MQTT with ThingSpeak
First let’s create an MQTT Device in ThingSpeak:
- Devices ->MQTT and add a new Device (add your channel in the new device list)
- Save the MQTT credentials (id,user,psw) to be used later
Then to change values of the fields we use the MQTT Publish:
```bash
mosquitto_pub -h "mqtt3.thingspeak.com" -p 1883 -u <MQTT_USER>
-P <MQTT_PSW> -i <MQTT_ID> -t "channels/<CHANNEL_ID>/publish"
-m "field1=XX&field2=YY&status=MQTTPUBLISH"
```
- &status=MQTTPUBLISH is a best practice to be sure that things go as planned
Is also possible to use MQTT to receive data(Subscribe)
# Node-RED
Visual tool for wiring the Internet of Things
The user creates flows by wiring together different nodes:
- I/O (serial port, tcp sockets, http, mqtt, files)
- Functions (built in or custom JavaScript functions)
- Advanced (execute programs, post a tweet)
All the changes applied to the flow need to be saved and loaded with the Deploy button. Javascript let you to have full control on the messages content.
You can search additional services through the extensions.
## Inject Node
Node that only has output and can be used to provide data
## Function node
Node that can run some JAVASCRIPT functions
## Debug Node
Print in output what it get as input
