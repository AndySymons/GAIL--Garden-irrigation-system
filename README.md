# GAIL - a garden irrigation system

-- PAGE UNDER CONSTRUCTİON -- 

A Home Assistant Blueprint for automations that operate a multi-zone irrigation system based on moisture sensors and the weather forecast.  

The setup is described in the following steps:
1. Build an irrigation system with an electrically controlled valve for each zone 
2. Connect the zone control valves to smart switches or a smart controller 
3. Install a soil moisture sensor for each zone
4. Install a weather forecast integration
5. Define Home Assistant helpers needed by the blueprint 
6. Install an optional logging file to track the operation of the irrigation automation 
7. Install the GAIL Garden Irrigation blueprint and required script
8. Create a garden irrigaton dashboard
9. Create an automation to actually do the work
10. Test the automation  

### 1. Build an irrigation system with an electrically controlled valve for each zone 

I am not going to discuss the plumbing of a garden irrigation system here because it will depend very much on your garden. There are plenty of YouTube videos on the subject. 
For example, I have four zones. My garden has three lawn areas, each with just one pop-up sprinkler, and the fourth zone is used for patio pots with low pressure mini-sprinklers. 

The minimum you need to automate an irrgiation system is one electric valve, or more generally, one per zone. There are several available, usually requiring at 24VAC at 1 Amp. 
I use four Hunter PGV Jar top valves connected to a manifold in a box buried in the garden. The control electrics are in the garage. 

![Hunter PGV jar-top valve](https://github.com/user-attachments/assets/c64ee78c-7eaf-4eb6-8e6b-d5897b7152b2)

You will need a 24V transformer. Usually, and certainly with my blueprint, only one zone is operated at a time, so the transformer only needs to be rated for one valve. 2A is recommended to be on the safe side. 


### 2. Connect the zone control valves to smart switches or a smart controller 

The blueprint accepts either switches or an Orbit B-Hyve controller. 

#### 2.1 Alternative 1: Connect the zone control valves to smart switches 
This blueprint is self-contained and equires no more than to connect the zone control valves to smart switches. I recommend just that for new installations. 
Any smart switch with the approriate rating for the valves will do. 

##### Examples
I have, for example, successfully used the Sonoff Pro 4-gang smart switch in other applications:

![SONOFF 4CH Pro smart switch](https://github.com/user-attachments/assets/a1ca0f06-cd0a-446b-877f-20ea2daf9847)

There are also several ESP32 relay boards on the market that can be programmed with ESPHone, if you prefer a more DIY approach. 

![Example ESP32 4-Channel relay board](https://github.com/user-attachments/assets/7ea246cf-850b-4a94-be98-2af3cf9959af)


#### 2.2. Alternative 2: Connect the zone control valves to ann Orbit B-Hyve smart controller (4 or 8 zones)
In my case I already had an Orbit B-Hyve 4-zone irrigation control system in place. There was no call to replace it, so I used it. 

![Screenshot 2024-12-06 at 13 14 06](https://github.com/user-attachments/assets/ad742b41-5ba2-449d-88ca-59a781acf3d6)


This blueprint only uses it as a smart switch, so its own program is turned off (but is nice to have as a fallback if needed). It is connected to Home Assistant using the  **Orbit B-Hyve** integration. 

![Screenshot 2024-12-06 at 13 00 06](https://github.com/user-attachments/assets/4ac0ce2d-df91-4370-bd64-8c0bb8fce13c)

This integration presents the zone switches to Home Assistant as domain type 'switch'. HOWEVER, if you turn on a B-Hyve zone like an ordinary switch, it automatically turns off after 10 minutes. For longer watering periods with this integration, it is necessary to use the methods'start watering' and 'end watering' instead.

The blueprint therefore incorporates a selector for the valve control type. The default is 'Switch'. If instead 'B-Hyve' is selected, then the blueprint calls the special 'start watering' and 'end watering' functions instead of switch on and off to ensure that the full watering time is available. 

### 3. Install a soil moisture sensor for each zone

Any sensor can be used that provides soil moisture as a percentage. 

##### Example
I am using Ecowitt WH51 Wireless Soil Moisture sensors and they have proved very reliable. 

![Ecowitt WH51 Wireless Soil Moisture sensor](https://github.com/user-attachments/assets/51e4182a-fd1e-4284-8983-d1496b79fbaa)

They transmit with the relatively low frequency of 925 MHz in North America or 868 MHz in Europe. This gives them a good range to transmit the length of a large garden. 

As well as a sensor per zone, you need a GW1100 gateway to connect up to eight of them to WiFi. 

![Ecowitt GW1100 Gateway](https://github.com/user-attachments/assets/a91fdb4e-b5a7-4a0a-9471-5c1a29eac211)

This is then conected to Home Assistant using the EcoWitt integration.

![Screenshot 2024-12-06 at 12 59 46](https://github.com/user-attachments/assets/d348435e-6ffc-48ff-942b-0bdc7dc8a657)


### 4. Install a weather forecast integration

You can use any weather forecast integration that provides the forecast precipitation for the following day in the usual format. They have a standard data interface for Home Assistant.
I use the Norwegian Meterologisk Institutt integration.

![Screenshot 2024-12-06 at 13 12 36](https://github.com/user-attachments/assets/80bd602d-23fc-4698-b1d3-18585f950a3e)


### 5. Define Home Assistant helpers needed by the blueprint  

The following are required for each zone, though you can share over some or all zones if you want to contrain those zones to the same values: 

| Name                     | Type         | Permitted values | Unit of measurement | Use                               |
|--------------------------|--------------|------------------|---------------------|-----------------------------------|
| Threshold soil moisture  | Input Number | 0 ... 100        | Percentage | The soil moisture percentage below which the zone will not be watered  | 
| Target soil moisture     | Input Number | 0 ... 100        | Percentage | The soil moisture percentage at which watering will stop   | 
| Maximum watering time    | Input Number | 0 ... 180        | Minutes    | The maximum time for which one watering session will last, regardless of the soil miosture level | 
| Default watering time    | Input Number | 0 ... 180        | Minutes    | The time for which one watering session will last if there is no available soil moisture sensor  | 

In addition a single timer helper is required, which the blueprint will use to time the watering (only one zone at a time is watered so onşt one timer is requıred).  


## 6. Install an optional logging file to track the operation of the irrigation automation 

If logging of the garden watering history is required, then 
1. Install my [logfile_entry script package](https://github.com/AndySymons/HEATHER-Home-Heating-Control-for-Home-Assistant-/blob/Version-2.x/Package%20scripts/logfile_entry.yaml) 
2. Define the logfile notification service using the [Home Assistant File integration](https://www.home-assistant.io/integrations/file/), which must therefore be installed first if you do not already have it.


## 7. Install the GAIL Garden Irrigation blueprint and required script 

-- Under construction -- 

## 8. Create a garden irrigaton dashboard

-- Under construction -- 


## 9. Create an automation to actually do the work

-- Under construction -- 

The final step is to create the automation that is actually going to control your irrigation system. 

In addition to the helpers already above, the blueprint takes the following as constant parameters

- Irrigation control valve type; switch (default) or B-Hyve (see above)
- Your weather forecast service, if applicable. If left blank then the weather forecast is not taken into account. 
- The threshold forecast precipitation deemed sufficient that no irrigation will be required, in millimetres (default 25). 
- The garden irrgiation logfile notification service, if applicable, otherwise blank

## 10. Test the automation

-- Under construction --  

---
If you find any of the ideas in this repository useful, please [Buy Me a Coffee](https://buymeacoffee.com/andysymons)
