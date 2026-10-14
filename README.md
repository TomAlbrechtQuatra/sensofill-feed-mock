# SensoFill Feed – test environment

This repository simulates the feed of **SensoFill**, the supplier of the fill-level sensors on our oil containers. In production this would be a REST API. For this assignment, the feed is served as static JSON files over a plain HTTP GET.

Treat these files as an external API: fetch them over HTTP from Odoo and do not modify them.

## Endpoints

| File | Meaning |
|---|---|
| `readings_batch_1.json` | Feed response on a first poll |
| `readings_batch_2.json` | Feed response on a subsequent poll, one week later |

Use the **raw** URL of the file, for example:

```
https://raw.githubusercontent.com/<account>/<repo>/main/readings_batch_1.json
```

The feed returns the most recent readings received by SensoFill. Consecutive polls may overlap.

**Simulating the next run:** let your integration process batch 1 first. Then change the configured URL to batch 2 and run the integration again.

## Response

```json
{
  "provider": "SensoFill",
  "feed_version": "1.2",
  "generated_at": "2026-10-15T00:00:00Z",
  "count": 68176,
  "readings": [
    {
      "entry_id": 100001,
      "sensor_id": "QS-0126",
      "measured_at": "2026-10-01T00:00:00Z",
      "fill_level_pct": 62.1,
      "battery_pct": 48
    }
  ]
}
```

| Field | Description |
|---|---|
| `generated_at` | Timestamp at which the feed was generated |
| `count` | Number of readings in this response |
| `entry_id` | Unique sequence number of the reading at SensoFill |
| `sensor_id` | Sensor ID as registered at SensoFill |
| `measured_at` | Timestamp of the measurement (ISO 8601) |
| `fill_level_pct` | Fill level of the container, in percent |
| `battery_pct` | Battery level of the sensor, in percent |

## Master data

`containers.csv` is an export from our current planning file. It contains the customers, their containers (with capacity in litres) and which sensor is mounted on which container. Load this data however you see fit: as demo data in your module, through an import, …

Columns: `customer_ref`, `customer_name`, `street`, `zip`, `city`, `container_ref`, `capacity_l`, `sensor_id`, `threshold_pct` (empty = default threshold).

## Questions

Anything unclear about the feed or the master data? Ask your internal customer.
