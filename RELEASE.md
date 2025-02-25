# RELEASE NOTES

## v0.5.0 - TBD

* Added additional exception handling to help identify connection and login errors.
* Added `is_connected()` function to test for a successful connection to the Powerwall.

## v0.4.0 - Cache Bypass Option and New Functions

* PyPI 0.4.0
* Added parameter to `poll()` to force call (ignore cache)
* Added `alerts()` function to return an array of device alerts.
* Added `get_reserve()` function to return battery reserve setting.
* Added `grid_status()` function to return state of grid.
* Added `system_status()` function to return system status.
* Added `battery_blocks()` function to return battery specific information.
* Expanded class to include settings for cache expiration (`pwcacheexpire`) and connection `timeout`.

```python
# Force Poll
pw.poll('/api/system_status/soe',force=True)
'{"percentage":100}'

# Powerwall Alerts
pw.alerts()
['PodCommissionTime', 'GridCodesWrite', 'GridCodesWrite', 'FWUpdateSucceeded', 'THC_w155_Backup_Genealogy_Updated', 'PINV_a067_overvoltageNeutralChassis', 'THC_w155_Backup_Genealogy_Updated', 'PINV_a067_overvoltageNeutralChassis', 'PVS_a018_MciStringB', 'SYNC_a001_SW_App_Boot']

# Battery Reserve Setting
pw.get_reserve()
20.0

# State of Grid
pw.grid_status()
'UP'
```

## v0.3.0 - Device Vitals Alerts and Attributes

* PyPI 0.3.0
* Added alerts and additional attributes from `vitals()` output.
* Note: API change to `vitals()` output for dependant systems.

## v0.2.0 - Tesla Protocol Buffer Scheme Update

* PyPI 0.2.0
* Breaking change to Protobuf schemea (PR #2) including:
* Files `tesla.proto` and `tesla_pb2.py`
* Impacted output from function `vitals()` and [examples/vitals.py](examples/vitals.py).

## v0.1.4 - Battery Level Percentage Scaling

* PyPI 0.1.4
* Changed "Network Scan" default timeout to 400ms for better detection.
* Added Tesla App style "Battery Level Percentage" Conversion option to `level()` to convert the level reading to the 95% scale used by the App. Ths converts the battery level percentage to be consistent with the Tesla App:

```python
>>> pw.level(scale=True)
39.971429212508326
>>> pw.level()
42.972857751882906
```

## v0.1.3 - Powerwall Temps

* PyPI 0.1.3
* Added `temp()` function to pull Powerwall temperatures.

```python
pw.temps(jsonformat=True)
```

```json
{
    "TETHC--2012170-25-E--TGxxxxxxxxxxxx": 17.5,
    "TETHC--3012170-05-B--TGxxxxxxxxxxxx": 17.700000000000003
}
```

## v0.1.2 - Error Handling and Proxy Stats

* PyPI 0.1.2
* Added better Error handling for calls to Powerwall with debug info for timeout and connection errors.
* Added timestamp stats to pypowerwall proxy server.py (via URI /stats and /stats/clear)

pyPowerwall Debug
```
DEBUG:pypowerwall [0.1.2]

DEBUG:loaded auth from cache file .powerwall
DEBUG:Starting new HTTPS connection (1): 10.0.1.2:443
DEBUG:ERROR Timeout waiting for Powerwall API https://10.0.1.2/api/devices/vitals
```

Proxy Stats
```json
{"pypowerwall": "0.1.2", "gets": 2, "errors": 3, "uri": {"/stats": 1, "/soe": 1}, "ts": 1641148636, "start": 1641148618, "clear": 1641148618}
```

## v0.1.1 - New System Info Functions

* PyPI 0.1.1
* Added stats to pypowerwall proxy server.py (via URI /stats and /stats/clear)
* Added Information Functions: `site_name()`, `version()`, `din()`, `uptime()`, and `status()`.

```python
     # Display System Info
     print("Site Name: %s - Firmware: %s - DIN: %s" % (pw.site_name(), pw.version(), pw.din()))
     print("System Uptime: %s\n" % pw.uptime())
```

## v0.1.0 - Vitals Data

* PyPI 0.1.0
* Added *protobuf* handling to support decoding the Powerwall Device Vitals data (requires protobuf package)
* Added function `vitals()` to pull Powerwall Device Vitals
* Added function `strings()` to pull data on solar panel strings (Voltage, Current, Power and State)

```python
     vitals = pw.vitals(jsonformat=False)
     strings = pw.strings(jsonformat=False, verbose=False)
```

## v0.0.3 - Binary Poll Function, Proxy Server and Simulator

* PyPI 0.0.3
* Added Proxy Server - Useful for metrics gathering tools like telegraf (see [proxy](proxy/)]).
* Added Powerwall Simulator - Mimics Powerwall Gateway responses for testing (see [pwsimulator](pwsimulator/)])
* Added raw binary poll capability to be able to pull *protobuf* formatted payloads like '/api/devices/vitals'.

```python
     payload = pw.poll('/api/devices/vitals')
```

## v0.0.2 - Scan Function

* PyPI 0.0.2
* pyPowerwall now has a network scan function to find the IP address of Powerwalls
```bash
# Scan Network for Powerwalls
python -m pypowerwall scan
```
Output Example:
```
pyPowerwall Network Scanner [0.0.2]
Scan local network for Tesla Powerwall Gateways

    Your network appears to be: 10.0.3.0/24

    Enter Network or press enter to use 10.0.3.0/24: 

    Running Scan...
      Host: 10.0.3.22 ... OPEN - Not a Powerwall
      Host: 10.0.3.45 ... OPEN - Found Powerwall 1234567-00-E--TG123456789ABC
      Done                           

Discovered 1 Powerwall Gateway
     10.0.1.45 [1234567-00-E--TG123456789ABC]
```

## v0.0.1 - Initial Release

* PyPI 0.0.1
* Initial Beta Release 0.0.1