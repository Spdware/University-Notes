## Why different protocols?
Different needs...
End-user cloud applications
- Display sensor data on mobile app, website dashboards
	 HTTP, Websockets...
Local sensor networks
- Transmit sensor data
	 (Usually) over 802.15.4 (zigbee), Bluetooth low energy BLE
HTTP is heavy to run on IoT devices, this is why we introduced CoAP
## HTTP methods in a nutshell
Client-server achitecture
Restful API methods
- GET: read resource
- POST: create new resource
- PUT: update resource
- DELETE: delete resource

![](https://i.imgur.com/vx1ZZBm.png)

When you know the location of the resources you use the Uniform Resource Identifiers(URI) to know what is the resource over the internet that you want to request.

![](https://i.imgur.com/1cyaw8F.png)

This same structure will be used in CoAP.
## How to send HTTP requests?
Several ways to send an HTTP request to a server:
- Via browser (https://api.thecatapi.com/v1/images/search)
- GUI clients: Postman (https://docs.api.getpostman.com)
- Code library (C, python, C++, Java, JavaScript...)
- Command line (with curl)
We will use curl as we can use the different way to do a request and not only the GET request.
- Request header + Response header
- -v option in curl include the headers
- Roughly 370 bytes for the request/response
**Too verbose!**
### HTTP Problems
- Long messages
- Resource-hungry
### Drawbacks in IoT
Target devices:
- Battery-powered
- ≈ 1$
- ≈ 100 KB Flash
- ≈ 100 KB RAM
**HTTP is not a good fit!**
# COnstrained Application Protocol (CoAP)
Fits to the small resources of IoT devices
RESTful calls
Only needs small memories and slow processor
Uses the UDP protocol
M2M support

![](https://i.imgur.com/lM2Wmlw.png)

It doesn't use the three hand handshake.
## CoAP Features
RESTful API
UDP datagram based
Resource **Discovery** mode
**Observe** Resources
CONfirmable/NON-Confirmable messages (CON/NON)
**Block transfer** mode
## CoAP in Action
Smart home scenario (based on https://github.com/fpalmese/IoT2023/tree/main/CoAPServer):
1. Discovery
2. Read the temperature once - > GET
3. Read every change of temperature -> Observe
4. Close the living room door -> PUT
5. Add a smart door -> POST

![](https://i.imgur.com/tFzokwC.png)
### CoAP Requests/Operations
- DISCOVERY: show available resources
- GET: read the value of a resource
- POST/PUT: change the value of a resource
- POST: create a new resource
- DELETE: delete a resource
- OBSERVE: obtain all new values of a resource
#### CoRE Resource Discovery
A special GET request which asks for the available resources
GET request directed to the resource /.well-known/core

![](https://i.imgur.com/o6XR6Wr.png)

With all the resources there is also the options of the resource.
- -o: the resource is observable and the server notify you each time it changes, similar to publish/subscribe in http. This is done to not waste the client resource
### Some Examples
- Read a resource value with GET:
	coap get coap://localhost:5683/dining_room/temperature
- Update a resource value with PUT (with URI query):
	coap put coap://localhost:5684/living_room/door?status=CLOSED
- Update a resource value with POST (with payload):
	coap post coap://localhost:5683/hello_post –p "hello"
- Create a resource with POST (with query and payload):
	coap post coap://localhost:5683/living_room?create -p "new_door"
- Delete a resource with DELETE
	coap delete coap://localhost:5683/living_room/door
### CoAP Response Codes
![](https://i.imgur.com/yTMJ4uB.png)

### Block Transfer Mode
Send a esponse into separate application messages
- If the resource is big, transfer it using several application messages
- If a block is lost? Retransmit only the single block (if request is CON)
This is done if the resource is big/there is the need to increase efficiency.
## CoAP vs. HTTP
![](https://i.imgur.com/I4Kt8xy.png)
# Wireshark - CoAP Packet Inspection
 Open source packet analyzer
- Traffic analysis
- Packet analysis
- Protocol analysis
- Sniffing
Process of capturing, decoding and analyzing network traffic
- What is the network traffic pattern
- How traffic flows among nodes
Suggestion is to collect from all the interfaces
Token match request and responses.
