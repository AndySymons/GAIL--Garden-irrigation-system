# GAIL - a garden irrigation system

-- Work in progress --

A Home Assistant Blueprint for automations that operate a multi-zone irrigation system based on moisture sensors and the weather forecast.  

### Hardware requirements 
1. An electrically-operated valve for each irrigation zone (I am not going to discuss plumbing here), connected to Home Assistant with smart switches OR an Orbit B-Hyve controller (note 1).
2. A soil moisture sensor for each zone (note 2). 

##### Note 1: Valve controller types
It is sufficient to connect the xone control valves to Home Assistant using smart switches, and I recommend that for new installations. I have successfully used the Sonoff Pro 4-gang smart switch in other applications, but any will do. 
This blueprint requires no further irrigation control system but in my case I already had an **Orbit B-Hyve** 4-zone system in place and there was no call to replace it. It is only used like a smart swtch but it is also nice to have it as a fallback if needed. However, if you turn on a B-Hyve zone like an ordinary switch, it automatically turns off after 10 minutes. The blueprint therefore incorporates a selector for the valve control type. The default is 'Switch'; if instead 'B-Hyve' is selected, then the blueprint calls the 'start watering' and 'end watering' functions available in the integration to ensure that the full watering time is available. 

##### Note 2: Soil moisture sensors 
Any sensor can be used that provides soil moisture as a percentage. I am using xxx and they have proved very reliable. As well as a sensor per zone, you need a gateway to connect it to WiFi and Home Assistant.    

### HA System requirements 
1. A weather forecast integration that provides the forecast precipitation for the following day in the usual format.
2. The Orbit B-Hyve integration if you are using a B-Hyve controller to operate the zone valves.  
3. The appropriate integration for the soil moisture sensors -- in my case xxx
4. The GAIL blueprint

## Helpers required 

The following are required for each zone, though you can share over some or all zones if you want to contrain those zones to the same values: 

| Name                     | Type         | Permitted values | Unit of measurement | Use                               |
|--------------------------|--------------|------------------|---------------------|-----------------------------------|
| Threshold soil moisture  | Input Number | 0 ... 100        | Percentage | The soil moisture percentage below which the zone will not be watered  | 
| Target soil moisture     | Input Number | 0 ... 100        | Percentage | The soil moisture percentage at which watering will stop   | 
| Maximum watering time    | Input Number | 0 ... 180        | Minutes    | The maximum time for which one watering session will last, regardless of the soil miosture level | 
| Default watering time    | Input Number | 0 ... 180        | Minutes    | The time for which one watering session will last if there is no available soil moisture sensor  | 

In addition a single timer helper is required, which the blueprint will use to time the watering (only one zone at a time is watered so onşt one timer is requıred).  

## Logging

If logging of the garden watering hiator is required, then 
- my [logfile_entry script package](https://github.com/AndySymons/HEATHER-Home-Heating-Control-for-Home-Assistant-/blob/Version-2.x/Package%20scripts/logfile_entry.yaml) must be installed
- the logfile notification service has to be defined using the [Home Assistant File integration](https://www.home-assistant.io/integrations/file/), which must therefore be installed first if you do not already have it.  

## Constant parameters 

In addition to the above, the blueprint takes the following as constant parameters

- Irrigation control valve type; switch (default) or B-Hyve (see above)
- Your weather forecast service, if applicable. If left blank then the weather forecast is not taken into account. 
- The threshold forecast precipitation deemed sufficient that no irrigation will be required, in millimetres (default 25). 
- The garden irrgiation logfile notification service, if applicable, otherwise blank

 


---
If you find any of the ideas in this repository useful, please [Buy Me a Coffee](https://buymeacoffee.com/andysymons)
