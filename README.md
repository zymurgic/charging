# Electric vehicle charging
## Chargepoint Services

None of this documentation is official, is subject to change at Chargepoint Services' whim.

### List of locations

```
HTTP GET https://www.cpsgenie.com/ds/map/locationsummaries
```
This will return a JSON formatted output, Chargepoint Services group charge points at a given location. In their nomenclature, this these are 'posts'.

It will only list locations that are publicly visible - the private schemes they run for companies like Microsoft are not listed, there may be hidden parameters to provide scheme identifiers
that may list them. Uncertain.

The GWR scheme locations (at a small number of railway stations) are made visibile if you provide a cookie as though you were logged in to their web site. If you are not logged in, the GWR scheme locations are not shown. If you are, they are. Why this behaviour is exhibited by Chargepoint Services for public charging is unknown. For private charging schemes (Microsoft,Oracle,NATS), it makes sense.

It will also return a number of outlets of each type:

| Field | Datatype | Description |
|------ | -------- | ----------- |
| LocationId | string | hexadecimal with hyphens - unique id for the location |
| LocationDescription | string | abbreviated address, unstructured, no postcodes |
| Latitude | real | in decimal degrees |
| Longitude | real | in decimal degrees |
| TotalChargepoints | int | |
| TotalChademo | int | |
| TotalCcs | int | |
| TotalRapidAc | int | |
| TotalFastAc | int | |
| TotalSlowAc | int | |
| InUseChademo | int | |
| InUseCcs | int | |
| InUseRapidAc | int | |
| InUseFastAc | int | |
| InUseSlowAc | int | |
| FailedChademo | int | |
| FailedCcs | int | |
| FailedRapidAc | int | |
| FailedFastAc | int | |
| FailedSlowAc | int | |
| RapidConnectors | int | |
| FastConnectors | int | |
| SlowConnectors | int | |
| PostIds | array | array of post identifier strings |


### Location details
```
HTTPS GET https://www.cpsgenie.com/ds/map/LocationDetailDialogContent/d92c9324-ff94-d711-21d0-f58001d34801/locationModal
```
returns an HTML formatted table of deatails about a given location ID.
locationModalTitle	This is the same as the LocationDescription, but with an added ' - ' then a postcode

There isn an HTML DIV for each the Post Ids at that location, the ID being the Post Id.
For multi-headed chargers, it's one row per simultaneously available outputs, so CCS and Chademo are grouped together on APT rapid chargers, because using the CCS will mean that the Chademo is unavailable, and vice-versa, but the Rapid AC is still available.

| Field | Description | 
| ------| ----------- |
| No	|connector number
| Type	|Type of connector: One of |
|	|	|
|	|43 Kw AC Cable<br>|50 Kw DC Chademo and Combo (CCS) Cable<br>7 Kw AC Socket|
| Cost	|is cost of charge, in text, no longer abstracted into 'units', but given in pounds sterling|
| Status |Realtime status of the charge outlet, seems to have two spans, one abbreviated, and one long : One of<br>Available<br>In Use |
	
Action	There is a startCharge link, more of that later.

### Remote start charge
For those users without a registered RFID tag, you can remote start a charge via the app. User must be logged in for this to work.
```
HTTPS GET https://www.cpsgenie.com/ds/map/StartCharge/chargepointId/connectorId
```

Obviously, you cannot remotely start a charge unless you are logged in, and the connector status is 'Available'
This seems to have a chargepointId being a location ID, and the connectorID being the 'No' valule from LocationDetailDialogCount

### Remote stop charge
For those users without a registered RFID tag, you can remote stop all charges under your login
```
HTTPS GET https://www.cpsgenie.com/ds/map/StopCharges
```
