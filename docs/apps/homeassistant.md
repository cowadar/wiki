---
title: Home Assistant
---

## Snippets

trigger:

- platform: state
  entity_id: switch.half_bath_light
  from: 'off'
  to: 'on'

---

- condition: state
  entity_id: light.study_cans_current_value
  state: 'off'

---

action:

- service: switch.turn_off
  target:
  entity_id:
  - switch.s_outside_lights
  - switch.outside_back_patio_light

---

alias: Estufa - 20h On, 4 off
description: ''
trigger:

- id: 'off'
  platform: time
  at:
  - '12:00:00'
  - '20:00:00'
  - '04:00:00'
- id: 'on'
  platform: time
  at: - '16:00:00' - '00:00:00' - '08:00:00'
  condition: []
  action:
- service: 'switch.turn\_{{ trigger.id }}'
  target:
  entity_id: switch.0x847127fffeae874f
  mode: single

---

So if you have a light, and it’s entity_id is light.living_room, access any of the properties outside the attributes (or state), would be:

{{ states.light.living_room.entity_id }} # for the entity_id 'light.living_room'
{{ states.light.living_room.domain }} # for the domain 'light'
{{ states.light.living_room.object_id }} # for the object_id 'living_room'
{{ states.light.living_room.name }} # for the friendly_name 'Living Room'
{{ states.light.living_room.last_updated }} # for the datetime object in UTC when it was last updated
{{ states.light.living_room.last_changed }} # for the datetime object in UTC when the state last changed

---

alias: Example Sunrise Sunset Light
description: ''
trigger:

- id: '100'
  platform: sun
  event: sunset
  offset: '-01:00:00'
- id: '30'
  platform: time
  at: '23:30:00'
- id: '0'
  platform: sun
  event: sunrise
  offset: '-02:00:00'
  condition: []
  action:
- service: light.turn_on
  target:
  entity_id: light.outdoor_front
  data:
  brightness_pct: '{{ trigger.id }}'
  mode: single

---

alias: activate light for 930pm till 10pm
description: ''
trigger:

- id: 'on'
  platform: time
  at: '21:30:00'
- id: 'off'
  platform: time
  at: '22:00:00'
  condition: []
  action:
- choose:
  - conditions:
    - "{{ trigger.id == 'on' }}"
    - "{{ is_state('input_boolean.garage_light', 'on') }}"
      sequence:
    - service: switch.turn_on
      target:
      entity_id: switch.m_garage_light
  - conditions: "{{ trigger.id == 'off' }}"
    sequence: - service: switch.turn_off
    target:
    entity_id: switch.m_garage_light
    default: []
    mode: single

---

trigger:
platform: template
value_template: "{{ states('input_datetime.test_last_timestamp')|as_datetime + timedelta(days = 10) > now() }}"

---

chopping off first and last character:

{% set ev = "'66.5'" %}
{{ ev | int }} # will = 0
{{ ev')[1:-1] | int }} # will = 66

---

- id: '1586455192037'
  alias: School - Robin Wake Music
  description: ''
  trigger:
  - platform: time
    at: '05:55:00'
    condition:
  - "{{ is_state('calendar.noschool', 'off') }}"
  - "{{ is_state('binary_sensor.workday_sensor', 'on') }}"
  - "{{ 1 <= now().isoweekday() <= 5 }}"
    action:
  - variables:
    entities: - switch.echo_spare_room - switch.robin_tree_1 - light.robin - light.robin_closet - light.robin_chandelier
    songs: - 'Cover me in Sunshine' - 'See it, say it, sign it by Jack Hartman' - 'overweight hedgehog' - 'where are you Christmas' - '90 days' - 'hurt by johnny cash' - 'Fishy on Me' - 'someone you loved' - 'frantic by Metallica' - 'Enter sandman' - 'We built this city' - 'Welcome to the Jungle'
  - service: homeassistant.turn_on
    target:
    entity_id: '{{ entities }}'
  - repeat:
    until: "{{ now().hour == 6 and now().minute == 30 }}"
    sequence: - service: notify.alexa_media
    data:
    data:
    type: announce
    message: "It is {{ now().strftime('%H:%M')}}, time To Wake up"
    target: - media_player.robin_echo - service: media_player.play_media
    target:
    entity_id: media_player.robin_echo
    data:
    media_content_type: AMAZON_MUSIC
    media_content_id: '{{ songs | random }}' - wait_for_trigger: - platform: state
    entity_id: media_player.robin_echo
    from: 'playing'
    mode: single

https://community.home-assistant.io/t/how-to-trigger-automation-if-another-automations-actions-ran-to-many-times-in-a-given-time-frame/384689/9

---

{% set month = now().month + 6 %}
{{ month }}
{{ month in [1,12] }} FALSE
{{ (now() + timedelta(days=150)).month }}
{{ (now() + timedelta(days=150)).month in [5,6,7,8,9] }} TRUE (in january)

---

{% set date_string = "[“Jan 23 2022”]" %}
{{ date_string }}
{{ as_timestamp(strptime(date_string[2:-2], "%b %d %Y")) | timestamp_local}}

---

Alternative to calculating duration
Up {{ states("sensor.ha_uptime") | as_datetime | relative_time }}

vs this

{% set boot = as_timestamp(states('sensor.ha_uptime'))|int %}
{% set duration = as_timestamp(utcnow())|int - boot %}
{{ timedelta(seconds=duration) }}

---

https://community.home-assistant.io/t/updating-templates-with-the-new-default-values-in-2021-10-x/346198#2-arguments

Using them as a function.
2 arguments
breakdown

{{ function(value_for_argument1, value_for_argument2) }}

examples

# returns acos of 1, if fails returns 0

{{ acos(1, 0) }}

# returns as_timestamp of 1, if fails returns 0

{{ as_timestamp(1, 0) }}

# returns asin of 1, if fails returns 0

{{ asin(1, 0) }}

# returns atan of 1, if fails returns 0

{{ atan(1, 0) }}

# returns cos of 1, if fails returns 0

{{ cos(1, 0) }}

# returns 1.0, if fails returns 0

{{ float('1', 0) }}

# returns sin of 1, if fails returns 0

{{ sin(1, 0) }}

# returns sqrt of 1, if fails returns 0

{{ sqrt(1, 0) }}

# returns tan of 1, if fails returns 0

{{ tan(1, 0) }}

===========================================================
3 arguments
breakdown

{{ function(value_for_argument1, value_for_argument2, value_for_argument3) }}

examples

# returns time @ 10 am, if fails returns 0

{{ strptime("10:00", "%H:%M", 0) }}

# returns log of 1 with base 10, if fails returns 0

{{ log(1, 10, 0) }}

# returns atan2 of 1 at 2, if fails returns 0

{{ atan2(1, 2, 0) }}

# returns rounds 1.23 down precision of 0, if fails returns 0

{{ round(1.43, "floor", 0) }}

===========================================================
3 arguments but omitting the 2nd argument
breakdown

{{ function(value_for_argument1, argument3 = value_for_argument3) }}

examples

# returns log of 1 with base e because base (2nd argument) defaults to e, if fails returns 0

{{ log(1, default = 0) }}

# returns rounds 1.43 to even with a precision of 0

# because function (2nd argument) defaults to round-to-even, if fails returns 0

{{ round(1.43, default = 0) }}

===========================================================
Using them as filters

2 arguments
breakdown

{{ value_for_argument1 | function(value_for_argument2) }}

examples

# returns as_timestamp of 1, if fails returns 0

{{ 1 | as_timestamp(0) }}

# returns 1.0, if fails returns 0

{{ '1' | float(0) }}

# returns fully formatted time string 1 second past

# midnight January 1st 1970, if fails returns 0

{{ 1 | timestamp_local(0) }}

# returns fully formatted time string 1 second past

# midnight January 1st 1970, if fails returns 0

# {{ 1 | timestamp_utc(0) }}

3 arguments
breakdown

{{ value_for_argument1 | function(value_for_argument2, value_for_argument3) }}

examples

# returns time @ 10 am, if fails returns 0

{{ "10:00" | strptime("%H:%M", 0) }}

# returns rounds 1.23 down precision of 0, if fails returns 0

{{ 1.43 | round("floor", 0) }}

===========================================================

3 arguments but omitting the 2nd argument
breakdown

{{ value_for_argument1 | function(argument3 = value_for_argument3) }}

examples

# returns rounds 1.23 down precision of 0, if fails returns 0

{{ 1.43 | round(default=0) }}

===========================================================

4 arguments
breakdown

{{ value_for_argument1 | function(value_for_argument2, value_for_argument3, value_for_argument4) }}

examples

# returns "00:00:01", which is 1 second past

# midnight January 1st 1970 in local time, if fails returns 0

{{ 1 | timestamp_custom("%H:%M:%S", True, 0) }}

===========================================================
4 arguments omitting the 3rd argument
breakdown

{{ value_for_argument1 | function(value_for_argument2, value_for_argument3, value_for_argument4) }}

examples

# returns "00:00:01", which is 1 second past

# midnight January 1st 1970 in local time (local_time defaults to True), if fails returns 0

{{ 1 | timestamp_custom("%H:%M:%S", default = 0) }}

---

Virtual Power Sensors based on on-off/% brightness

https://community.home-assistant.io/t/powercalc-virtual-power-sensors/318515

---

https://community.home-assistant.io/t/only-allow-automation-to-trigger-once-a-day/390097

- condition: template
  # Only run if more than 6 hours (21,600 sec) since it last ran
  value_template: '{{(as_timestamp(now()) - as_timestamp(state_attr("automation.<automation ID>", "last_triggered") | default(0)) | int > 21600 )}}'

---

{% set directions = [ 'N', 'NNE', 'NE', 'ENE', 'E', 'ESE', 'SE', 'SSE', 'S', 'SSW', 'SW', 'WSW', 'W', 'WNW', 'NW', 'NNW', 'N'] -%}
{{ directions[(states('sensor.dwd_wind_bearing') | int(0) / 22.5) | round(0) | int] }}

---

Could somebody help me trying to understand how could I extract information from curl query bellow.
Im intrested only the price value

curl -X GET “https://dashboard.elering.ee/api/nps/price/EE/current 3” -H “accept: /”

sensor:

- platform: rest
  resource: https://dashboard.elering.ee/api/nps/price/EE/current
  name: Elering
  value_template: "{{ value_json['data'][0]['price'] }}"
  unit_of_measurement: '€'

---

id: '1643570261869'
alias: play tv in the kitchen in the morning when sensor sees me
description: ''
trigger:

- type: motion
  platform: device
  device_id: 7a19b2c869a28edaf40ba6c0d0d1a925
  entity_id: binary_sensor.kitchen_sensor_motion
  domain: binary_sensor
  condition:
- condition: time
  after: '05:00:00'
  before: '09:30:00'
  action:
- service: remote.send_command
  data:
  device: kitchen tv
  command: power
  target:
  device_id: 1276592bc45474738b30066b2516c4f0
- service: androidtv.adb_command
  data:
  command: am force-stop de.cyberdream.iptv.tv.player
  target:
  device_id: 1276592bc45474738b30066b2516c4f0
- delay:
  hours: 0
  minutes: 0
  seconds: 0
  milliseconds: 500
- service: media_player.select_source
  data:
  source: de.cyberdream.iptv.tv.player
  target:
  entity_id: media_player.kitchen_gtv
  mode: restart
  Basically, you want it to execute its actions only if the value of the automation’s last_triggered is before 05:00 today. If the value is after 05:00 that means it has already triggered once today.

The condition to achieve this is simple:

condition:

- condition: time
  after: '05:00:00'
  before: '09:30:00'
- "{{ state_attr(this.entity_id, 'last_triggered') | default(today_at(), true) < today_at('05:00') }}"

---

TIME
https://community.home-assistant.io/t/the-epic-time-conversion-and-manipulation-thread/85786/3

as_timestamp() - converts a properly formatted date/time string to a Unix Epoch representation.
timestamp_custom() - converts a UNIX Epoch timestamp into a custom date/time string = unix_epoch_timestamp|timestamp_custom(format)

Directive Meaning Example Notes

%a Weekday as locale’s abbreviated name.
Sun, Mon, …, Sat (en_US);
So, Mo, …, Sa (de_DE)(1)
%A Weekday as locale’s full name.
Sunday, Monday, …, Saturday (en_US);
Sonntag, Montag, …, Samstag (de_DE)
%w Weekday as a decimal number, where 0 is Sunday and 6 is Saturday. 0, 1, …, 6
%d Day of the month as a zero-padded decimal number. 01, 02, …, 31
%-d Day of the month as a decimal number. 1, 2, ... 31
%b Month as locale’s abbreviated name.
Jan, Feb, …, Dec (en_US);
Jan, Feb, …, Dez (de_DE)
%B Month as locale’s full name.
January, February, …, December (en_US);
Januar, Februar, …, Dezember (de_DE)
%m Month as a zero-padded decimal number. 01, 02, …, 12
%-m Month as a decimal number. 1, 2, ... 12
%y Year without century as a zero-padded decimal number. 00, 01, …, 99
%Y Year with century as a decimal number. 0001, 0002, …, 2013, 2014, …, 9998, 9999
%H Hour (24-hour clock) as a zero-padded decimal number. 00, 01, …, 23
%-H Hour (24-hour clock) as a decimal number. 0, 1, 2, ... 23
%I Hour (12-hour clock) as a zero-padded decimal number. 01, 02, …, 12
%-I Hour (12-hour clock) as a decimal number. 1, 2, ... 12
%p Locale’s equivalent of either AM or PM.
AM, PM (en_US);
am, pm (de_DE)
%M Minute as a zero-padded decimal number. 00, 01, …, 59
%-M Minute as a decimal number. 0, 1, 2, .. 59 (can go to 61 to account for leap seconds)
%S Second as a zero-padded decimal number. 00, 01, …, 59 (can go to 61 to account for leap seconds)
%-S Second as a decimal number. 0, 1, 2, .. 59
%f Microsecond as a decimal number, zero-padded on the left. 000000, 000001, …, 999999
%z UTC offset in the form +HHMM or -HHMM (empty string if the the object is naive). (empty), +0000, -0400, +1030
%Z Time zone name (empty string if the object is naive). (empty), UTC, EST, CST
%j Day of the year as a zero-padded decimal number. 001, 002, …, 366
%-j Day of the year as a decimal number. 1, 2, ... 366
%U Week number of the year (Sunday as the first day of the week) as a zero padded decimal number. All days in a new year preceding the first Sunday are considered to be in week 0. 00, 01, …, 53
%W Week number of the year (Monday as the first day of the week) as a decimal number. All days in a new year preceding the first Monday are considered to be in week 0. 00, 01, …, 53
%c Locale’s appropriate date and time representation.
Tue Aug 16 21:30:00 1988 (en_US);
Di 16 Aug 21:30:00 1988 (de_DE)
%x Locale’s appropriate date representation.
08/16/88 (None);
08/16/1988 (en_US);
16.08.1988 (de_DE)
%X Locale’s appropriate time representation.
21:30:00 (en_US);
21:30:00 (de_DE)
%% A literal '%' character. %

The following work also but they aren't listed in the official python documentation:

%D date in month/day/year (ex Jan 1st, 2008 = 01/01/08) ?
%s date & time as a UNIX Epoch timestamp ?
%C century ??
%e same as %d??
%F date in year-month-day
%g year in what format? same as %y?
%G same as %Y?
%h abbreviated month. same as %b?
%k same as %-H or %-I?
%l " " "
%P Locale’s equivalent of either am or pm like %p above but lowercase ?
%r time as hour:minute:second AM/PM
%R time as hour:minute
%T time as hour:minute:second
%u UTC hour
%V week number of the year. same as %U or %W ??

For the items marked with a ? without further testing I can't figure out which value it is.

--

The following flag characters are permitted between the '%' character and the conversion specifier character:

\_ (underscore) Pad a numeric result string with spaces.

-      (dash) Do not pad a numeric result string.

0 Pad a numeric result string with zeros even if the conversion specifier character uses space-padding by default.

^ Convert alphabetic characters in result string to uppercase.

# Swap the case of the result string. (This flag works only with certain conversion specifier characters, and of these, it is only really useful with %Z.)

---

Trigger on a counter. https://community.home-assistant.io/t/how-to-create-a-trigger-with-every-counter-increment/395059

- alias: Google TV Counter
  trigger:
  - platform: state
    entity_id: counter.google_tv_counter
    to:
    condition: "{{ trigger.from_state.state | int < trigger.to_state.state | int }}"

---

{{ states.light.study_cans_current_value.last_changed }}
{{ states.light.study_cans_current_value.last_changed | as_timestamp }}
{{ ((utcnow() | as_timestamp - (states.light.study_cans_current_value.last_changed | as_timestamp)) / 60) | round(0) }}
{{ as_timestamp(20220415, 0) }}

{{ as_local(now() | as_timestamp | as_datetime) }}
{{ states('input_number.zone1') | int }}
{{ as_local( (now() | as_timestamp + states('input_number.zone1') | float * 3600 )| as_datetime ) }}

{{ states('sensor.sunrise') }}
{{ as_local((states('sensor.sunrise') | as_timestamp + 3600|int ) | as_datetime)}}
{{ as_local((states('sensor.sunrise') | as_timestamp + 5400|int ) | as_datetime)}}
{{ as_local((states('sensor.sunrise') | as_timestamp + 9000|int ) | as_datetime)}}
{{ as_local((states('sensor.sunrise') | as_timestamp + 10800|int ) | as_datetime)}}
{{ as_local((states('sensor.sunrise') | as_timestamp + 14400|int ) | as_datetime)}}
{{ as_local((states('sensor.sunrise') | as_timestamp + 16200|int ) | as_datetime)}}
{{ as_local((states('sensor.sunrise') | as_timestamp + 19800|int ) | as_datetime)}}

---

1.- Create a sensor which accounts for the time some binary sensor is 'on’

sensor:

- platform: history_stats
  name: Time Boiler On
  entity_id: binary_sensor.led_boiler
  state: 'on'
  type: time
  start: '{{ now().replace(hour=0).replace(minute=0).replace(second=0) }}'
  end: '{{ now() }}'

  2.- Create a utility meter to reset the values daily, weekly, monthly,…

utility_meter:
boiler_daily_usage:
source: sensor.time_boiler_on
cycle: daily

3.- Customize the utility so that you can benefit from the long-term statistics

homeassistant:
[...]
customize: # Add an entry for each entity that you want to overwrite.
sensor.boiler_daily_usage:
state_class: measurement

---

https://community.home-assistant.io/t/statistics-fix-issue-large-amount-of-data/419728

For displaying data, I used the SQLite Web Add-on (GitHub - hassio-addons/addon-sqlite-web: SQLite Web - Home Assistant Community Add-ons) :

```sql
SELECT CASE WHEN entity_id IS NULL
THEN 'Orphaned'
ELSE 'OK'
END Issue,
entity_id,
statistic_id
FROM ( SELECT b.entity_id, a.statistic_id FROM statistics_meta a
LEFT JOIN 'states' b
ON a.statistic_id = b.entity_id
GROUP BY a.statistic_id )
WHERE
entity_id IS NULL);
```

```sql
DELETE FROM "statistics_meta"
WHERE id IN ( SELECT id FROM ( SELECT a.id, b.entity_id, a.statistic_id
FROM statistics_meta a
LEFT JOIN 'states' b
ON a.statistic_id = b.entity_id
GROUP BY a.statistic_id )
WHERE entity_id IS NULL);
```

## /_ NULL means the statistic_id is orphaned _/

now() inside a template will cause the template to update on the minute, no rate_limit.
entities inside a template will cause to update when the entity changes state, no rate_limit.
using states without a domain will cause the template to update on all state changes, however it’s rate limited to 1 update a minute.
using states.<domain> will cause the template to update on all state changes, however it’s rate limited to 1 update a second

---

https://community.home-assistant.io/t/dew-point-temperature-sensor-my-configuration-yaml/432445

Dew from temperature/humidity sensor.

sensor:

- platform: template
  sensors:
  temp_dew_XY:
  value_template: >-
  {% set T, RH = states('sensor.temperature_X'), states('sensor.humidity_Y') %}
  {% if not T or RH == 'unknown' -%}
  'unknown'
  {%- else %}
  {% set T = T | float %}
  {% set RH = RH | float %}
  {% set a = 6.1121 | float %}
  {% set b = 18.678 | float %}
  {% set c = 257.14 | float %}
  {% set d = 234.5 | float %}
  {% set e = 2.718281828 | float %}

          {% set P = a*e**((b-T/d)*(T/(c+T))) | float %}
          {% set gamma = log((RH / 100)*e**((b-T/d)*(T/(c+T)))) | float %}
          {% set dew_temp = ((c * gamma) / (b - gamma))|round(3) %}
          {{ dew_temp}}
        {% endif %}
      unit_of_measurement: "°C"
      friendly_name: "Dew Temperature"

---

entities:
battery:
consumption: sensor.daily_kwh_from_battery
production: sensor.daily_kwh_to_battery
battery_charge: sensor.powerwall_charge
grid:
consumption: sensor.daily_kwh_from_grid
production: sensor.daily_kwh_to_grid
solar: sensor.daily_kwh_produced

---
