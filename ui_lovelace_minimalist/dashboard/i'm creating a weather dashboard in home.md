# WeatherFlow Tempest Dashboard Configuration

Below is the complete YAML configuration for the WeatherFlow Tempest dashboard using your sensor entities. Create this as a new dashboard in your Home Assistant configuration:

```yaml
title: WeatherFlow Tempest Weather Dashboard
views:
  - title: Weather
    path: weather
    icon: mdi:weather-sunny
    badges: []
    cards:
      # Current Weather Overview
      - type: vertical-stack
        cards:
          - type: weather-forecast
            entity: weather.forecast
            forecast_type: daily
            show_forecast: true
          
          - type: entities
            title: Current Conditions
            entities:
              - entity: sensor.st_00174194_temperature
                name: Temperature
                icon: mdi:thermometer
              - entity: sensor.st_00174194_feels_like
                name: Feels Like
                icon: mdi:thermometer-lines
              - entity: sensor.st_00174194_humidity
                name: Humidity
                icon: mdi:water-percent
              - entity: sensor.st_00174194_dew_point
                name: Dew Point
                icon: mdi:water-outline
              - entity: sensor.st_00174194_air_pressure
                name: Air Pressure
                icon: mdi:gauge
              - entity: sensor.st_00174194_wet_bulb_temperature
                name: Wet Bulb Temperature
                icon: mdi:thermometer-water
          
      # Temperature Section
      - type: vertical-stack
        cards:
          - type: custom:mini-graph-card
            name: Temperature
            entities:
              - entity: sensor.st_00174194_temperature
                name: Temperature
              - entity: sensor.st_00174194_feels_like
                name: Feels Like
            hours_to_show: 24
            points_per_hour: 2
            line_width: 2
            show:
              labels: true
              points: false
              legend: true
              fill: fade
          
          - type: custom:mini-graph-card
            name: Humidity
            entities:
              - entity: sensor.st_00174194_humidity
                name: Humidity
            hours_to_show: 24
            points_per_hour: 2
            line_width: 2
            show:
              labels: true
              legend: true
              fill: fade
      
      # Wind Section
      - type: vertical-stack
        cards:
          - type: custom:windrose-card
            title: Wind Rose
            hours_to_show: 24
            direction:
              entity: sensor.st_00174194_wind_direction
              name: Wind Direction
            speed:
              entity: sensor.st_00174194_wind_speed
              name: Wind Speed
            calm_percentage:
              show: true
              threshold: 0.5
            beaufort:
              show: true
              colors: true
            size: 200
          
          - type: custom:mini-graph-card
            name: Wind Speed and Gust
            entities:
              - entity: sensor.st_00174194_wind_speed
                name: Wind Speed
              - entity: sensor.st_00174194_wind_gust
                name: Wind Gust
              - entity: sensor.st_00174194_wind_lull
                name: Wind Lull
            hours_to_show: 24
            points_per_hour: 2
            line_width: 2
            show:
              labels: true
              legend: true
              fill: false
          
          - type: gauge
            entity: sensor.st_00174194_wind_speed
            name: Current Wind Speed
            min: 0
            max: 40
            severity:
              green: 0
              yellow: 15
              red: 25

      # Precipitation Section
      - type: vertical-stack
        cards:
          - type: custom:mini-graph-card
            name: Precipitation
            entities:
              - entity: sensor.st_00174194_precipitation
                name: Precipitation
            hours_to_show: 24
            points_per_hour: 2
            line_width: 2
            show:
              labels: true
              legend: true
              fill: fade
          
          - type: entities
            title: Precipitation Details
            entities:
              - entity: sensor.st_00174194_precipitation
                name: Precipitation
              - entity: sensor.st_00174194_precipitation_intensity
                name: Intensity
              - entity: sensor.st_00174194_precipitation_type
                name: Type

      # Lightning Section
      - type: entities
        title: Lightning
        entities:
          - entity: sensor.st_00174194_lightning_count
            name: Lightning Count
          - entity: sensor.st_00174194_lightning_average_distance
            name: Avg. Distance

      # Light and UV Section
      - type: vertical-stack
        cards:
          - type: entities
            title: Light and UV
            entities:
              - entity: sensor.st_00174194_uv_index
                name: UV Index
              - entity: sensor.st_00174194_illuminance
                name: Illuminance
              - entity: sensor.st_00174194_irradiance
                name: Irradiance
          
          - type: gauge
            name: UV Index
            entity: sensor.st_00174194_uv_index
            min: 0
            max: 11
            severity:
              green: 0
              yellow: 3
              red: 6

      # Device Status
      - type: entities
        title: System Status
        entities:
          - entity: sensor.st_00174194_battery_voltage
            name: Tempest Battery Voltage
          - entity: sensor.st_00174194_signal_strength
            name: Tempest Signal Strength
          - entity: sensor.st_00174194_uptime
            name: Tempest Uptime
          - entity: sensor.hb_00175707_signal_strength
            name: Hub Signal Strength
          - entity: sensor.hb_00175707_uptime
            name: Hub Uptime
```

## Installation Instructions

1. Make sure you have the following custom cards installed via HACS:
   - mini-graph-card
   - windrose-card

2. Create a new dashboard (for example, `weatherflow.yaml`) in your Home Assistant configuration directory.

3. Copy the above YAML configuration into your dashboard file.

4. Make sure you have the WeatherFlow integration properly set up and all the sensors are available.

5. You may need to create a weather entity for the forecast. If you don't have one already, add this to your `configuration.yaml`:

```yaml
weather:
  - platform: template
    name: "Forecast"
    condition_template: "sunny"
    temperature_template: "{{ states('sensor.st_00174194_temperature') | float }}"
    humidity_template: "{{ states('sensor.st_00174194_humidity') | int }}"
    pressure_template: "{{ states('sensor.st_00174194_air_pressure') | float }}"
    wind_speed_template: "{{ states('sensor.st_00174194_wind_speed') | float }}"
    wind_bearing_template: "{{ states('sensor.st_00174194_wind_direction') | int }}"
```

6. After creating the dashboard, restart Home Assistant to apply all changes.

This dashboard contains all the sections you would need:
- Current weather overview
- Temperature and humidity graphs
- Wind rose for directional data
- Wind speed graphs
- Precipitation details
- Lightning information
- Light/UV measurements
- System status

All of these use your exact sensor entity IDs as provided in your list.
