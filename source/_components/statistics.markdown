---
layout: page
title: "Statistics Sensor"
description: "Instructions on how to integrate statistical sensors into Home Assistant."
date: 2016-09-28 12:10
sidebar: true
comments: false
sharing: true
footer: true
logo: home-assistant.png
ha_category: Utility
ha_iot_class: Local Polling
ha_release: "0.30"
ha_qa_scale: internal
redirect_from:
 - /components/sensor.statistics/
---

The `statistics` sensor platform consumes the state from other sensors. It exports the `mean` value as state and the following values as attributes: `count`, `mean`, `median`, `stdev`, `variance`, `total`, `min`, `max`, `min_age`, `max_age`, `change`, `average_change` and `change_rate`. If it's a binary sensor then only state changes are counted.

If you are running the [recorder](/components/recorder/) component, on startup the data is read from the database. So after a restart of the platform, you will immediately have data available. If you're using the [history](/components/history/) component, this will automatically also start the `recorder` component on startup.
If you are *not* running the `recorder` component, it can take time till the sensor starts to work because a couple of attributes need more than one value to do the calculation.

## {% linkable_title Configuration %}

To enable the statistics sensor, add the following lines to your `configuration.yaml`:

```yaml
# enable the recorder component (optional)
recorder:

# Example configuration.yaml entry
sensor:
  - platform: statistics
    entity_id: sensor.cpu
  - platform: statistics
    entity_id: binary_sensor.movement
    max_age:
      minutes: 30
```

{% configuration %}
entity_id:
  description: The entity to monitor. Only [sensors](/components/sensor/) and [binary sensor](/components/binary_sensor/).
  required: true
  type: string
name:
  description: Name of the sensor to use in the frontend.
  required: false
  default: Stats
  type: string
sampling_size:
  description: Size of the sampling. If the limit is reached then the values are rotated.
  required: false
  default: 20
  type: integer
max_age:
  description: Maximum age of measurements. Setting this to a time interval will cause older values to be discarded.
  required: false
  type: time
precision:
  description: Defines the precision of the calculated values, through the argument of round().
  required: false
  default: 2
  type: integer
{% endconfiguration %}

<p class='img'>
  <img src='{{site_root}}/images/screenshots/stats-sensor.png' />
</p>
