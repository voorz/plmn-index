# PLMN-Index

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![PLMNs](https://img.shields.io/badge/PLMNs-3%2C386-green.svg)
![Operators](https://img.shields.io/badge/operators-3%2C550-green.svg)
![Countries](https://img.shields.io/badge/countries-229-green.svg)
![Icons](https://img.shields.io/badge/icons-650-green.svg)
![Data Size](https://img.shields.io/badge/all.json-1.7_MB-orange.svg)

A consolidated, structured PLMN (Public Land Mobile Network) database aggregating MCC/MNC operator data from multiple open sources. Designed for O(1) lookup performance with both single-PLMN and bulk retrieval modes.

---

## Overview

PLMN-Index merges data from three upstream sources into a unified, de-duplicated JSON dataset. Each PLMN is identified by a `{MCC}-{MNC}` key and contains full operator metadata, optional sub-operator (MVNO) information with GID matching, carrier icons, and country details.

| Metric | Count |
|---|---|
| PLMN entries | 3,386 |
| Operators | 3,550 |
| Operators with icons | 650 |
| Operators with subs (MVNOs) | 645 |
| Sub-operators | 1,028 |
| Countries | 229 |
| Country flag PNGs | 231 |
| Operator icon sets | 118 |

---

## Data Sources

All data is consolidated from the following sources. We gratefully acknowledge their contributors.

| Source | Link | Provides |
|---|---|---|
| **mcc-mnc.com** | [mcc-mnc.com](https://mcc-mnc.com) | MCC, MNC, brand, operator, status, bands, type, country, phone country codes |
| **Wikipedia** | [Mobile country code](https://en.wikipedia.org/wiki/Mobile_country_code) | MCC/MNC operator data (additional records and updates) |
| **iebb/NekokoLPA2** | [GitHub](https://github.com/iebb/NekokoLPA2) | Operator icons, GID1/GID2, profile names, sub-operators, region info |



---

## Usage

### Option 1: Single PLMN Lookup (Recommended for low-latency)

Fetch an individual PLMN file by constructing the URL from MCC and MNC:

```
GET https://raw.githubusercontent.com/voorz/plmn-index/main/plmn/list/{mcc}-{mnc}.json
```

**Example:** Look up PLMN `310-260` (T-Mobile US):

```bash
curl -s https://raw.githubusercontent.com/voorz/plmn-index/main/plmn/list/310-260.json
```

```json
{
  "mcc": "310",
  "mnc": "260",
  "country": {
    "name": "United States of America",
    "iso": "US",
    "code": "1",
    "region": "North America"
  },
  "operators": [
    {
      "brand": "T-Mobile",
      "operator": "T-Mobile USA, Inc.",
      "status": "Operational",
      "type": "National",
      "bands": "GSM 1900 / UMTS 1700 / UMTS 1900 / UMTS 2100 / LTE 700 / LTE 1700 / LTE 1900 / LTE 2500 / 5G 600 / 5G 2500 / 5G 39000"
    }
  ]
}
```

### Field Reference

#### PLMN-level fields

| Field | Type | Description |
|---|---|---|
| `mcc` | `string` | Mobile Country Code (3 digits, ITU-T E.212) |
| `mnc` | `string` | Mobile Network Code (2–3 digits) |
| `country` | `object` | Country information (see below) |
| `operators` | `array` | List of operators on this PLMN (see below) |

#### Country object

| Field | Type | Description |
|---|---|---|
| `name` | `string` | Country name |
| `iso` | `string` | ISO 3166-1 alpha-2 code |
| `code` | `string` | International dialing code |
| `region` | `string` | Geographic region (Europe, Asia, etc.) |

#### Operator object

| Field | Type | Required | Description |
|---|---|---|---|
| `brand` | `string` | Yes | Consumer-facing brand name |
| `operator` | `string` | Yes | Legal entity name |
| `status` | `string` | Yes | Operational status (Operational, Not operational, Unknown, etc.) |
| `type` | `string` | Yes | Network type (National, International, Test) |
| `bands` | `string` | No | Frequency bands |
| `icon` | `string` | No | Icon filename (without extension) |
| `icon_scope` | `string` | No | MCC scope for icon lookup |
| `subs` | `array` | No | Sub-operators / MVNOs (see below) |

#### Sub-operator object

| Field | Type | Description |
|---|---|---|
| `brand` | `string` | Sub-brand display name |
| `names` | `array` | Lowercase matching names for SPN lookup |
| `gid1` | `string` | Group Identifier Level 1 (hex) |
| `gid2` | `string` | Group Identifier Level 2 (hex) |
| `profile_names` | `array` | SIM profile names |
| `icon` | `string` | Sub-operator specific icon (optional) |
| `icon_scope` | `string` | Icon scope override (optional) |

---


## License

[MIT](LICENSE)

---

## Acknowledgements

This project is built upon the work of the following communities:

- [iebb](https://github.com/iebb) — [NekokoLPA2](https://github.com/iebb/NekokoLPA2)
- [mcc-mnc.com](https://mcc-mnc.com) — MCC/MNC database
- [Wikipedia](https://en.wikipedia.org/wiki/Mobile_country_code) — Mobile Country Code reference
