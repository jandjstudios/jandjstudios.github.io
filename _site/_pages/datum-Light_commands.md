



{::options parse_block_html="true" /}

<details><summary markdown="span">get /blank</summary>

```json

``` 
</details>

---

<details><summary markdown="span">get /device</summary>

```json
{
  "manufacturer": "J&J Studios LLC",
  "product": "datum-Light",
  "hardwareVersion": 1,
  "firmwareVersion": 1,
  "protocolVersion": 1,
  "UUID": "00-04-A3-0B-00-12-26-62"
}
``` 
</details>

---

<details><summary markdown="span">get /config</summary>

```json
{
  "friendlyName": "friendlyName",
  "reportRate": 5,
  "automaticReporting": false,
  "compactReport": false
}
``` 
</details>

---

<details><summary markdown="span">get /options</summary>

```json
{
  "friendlyName": {
    "minimum length": 0,
    "maximum length": 32
  },
  "reportRate": {
    "minimum": 0,
    "maximum": 100
  },
  "automaticReporting": ["true", "false"],
  "compactReport": ["true", "false"]
}
``` 
</details>

---

<details><summary markdown="span">get /sensors/color/device</summary>

```json
  {
    "manufacturer": "Avago Technologies",
    "model": "APDS9960",
    "category": "light",
    "type": "color"
  }
``` 
</details>

---
<details><summary markdown="span">get /sensors/color/config</summary>

```json
  {
    "enabled": true,
    "units": "counts",
    "gain": 16,
    "integrationCycles": 64,
    "filterType": "mean",
    "sampleRate": 20,
    "dataRate": 10
  }
``` 
</details>

---
<details><summary markdown="span">get /sensors/color/options</summary>

```json
  {
    "enabled": ["true", "false"],
    "units": ["counts", "normalized"],
    "gain": [1, 4, 16, 64],
    "integrationCycles": {
      "minimum": 1,
      "maximum": 256
    },
    "filterType": ["none", "min", "max", "mean", "RMS", "median"],
    "sampleRate": {
      "minimum": 0,
      "maximum": 250
    },
    "dataRate": {
      "minimum": 0,
      "maximum": 250
    }
  }
  ``` 
</details>

---
<details><summary markdown="span">get /sensors/proximity/device</summary>

```json
  {
    "manufacturer": "Avago Technologies",
    "model": "APDS9960",
    "category": "light",
    "type": "proximity"
  }

``` 
</details>

---



<details><summary markdown="span">get /sensors/proximity/config</summary>

```json
  {
    "enabled": true,
    "units": "counts",
    "gain": 1,
    "LEDstrength": "25 mA",
    "filterType": "mean",
    "sampleRate": 20,
    "dataRate": 10
  }
``` 
</details>

---


<details><summary markdown="span">get /sensors/proximity/options</summary>

```json  
  {
    "enabled": ["true", "false"],
    "units": ["counts", "normalized"],
    "gain": [1, 2, 4, 8],
    "LEDstrength": ["12.5 mA", "25 mA", "50 mA", "100 mA"],
    "filterType": ["none", "min", "max", "mean", "RMS", "median"],
    "sampleRate": {
      "minimum": 0,
      "maximum": 250
    },
    "dataRate": {
      "minimum": 0,
      "maximum": 250
    }
  }
``` 
</details>