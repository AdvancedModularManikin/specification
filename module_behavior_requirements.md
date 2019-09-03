# AMM Module Behavioral Requirements

## Overview
To ensure module interoperability, Modules must adhere to a set of Behavioral Requirements along with the
 [Configuration](configuration_data_model.md) and [Operational Data](operational_data_model.md) Models.
The Behavioral Requirements ensure a collection of connected AMM Modules will perform as expected, even with individual
 Modules produced by many different manufacturers.
This document defines those Behavior Requirements, breaking them down into three overall domains: Startup,
 Configuration, Operation.
 
## Module Connectivity
AMM uses the [DDSI-RTPS](https://www.omg.org/spec/DDSI-RTPS/About-DDSI-RTPS/) (version 2.3) protocol for all data
 transport between modules. 
 
### Networking
As an AMM Module boots, it needs to establish a network connection in order to communicate with any other Modules.
Because DDSI-RTPS is used as the underlying transport for data within AMM, Modules need to connect via UDP over
 IP(v4) and accept UDP broadcasts.
 
#### IP Addressing
IP addressing is usually managed via DHCP. Most modules will be simple DHCP clients.
 
If there are no Segment Modules connected and powered via PoE, IP addressing may be managed out of scope of AMM, such
 as by a commodity networking product.
Modules that are purely implemented in software may rely on their host OS for IP addressing.

#### DHCP Server
If the AMM system includes physical Segment Modules connected via ethernet and powered via PoE, the DHCP server must
 be managed by the PoE PSE device in order to facilitate power consumption monitoring.
In the reference implementation manikin, the Network Controller assumes this responsibility.

### Time Synchronization
Because a simulation is run over a distributed network, syncing time between modules is important.
AMM relies on the widely-used NTP standard for this.

#### NTP Server
In the case that an instance of an AMM network doesn’t have internet access, the computer acting as network router
 shall rely on its undisciplined local clock as the authoritative source.
 
### DDS Configuration
Once an AMM Module has finished booting, has established a network connection, and synchronized its time, it needs to
 establish a DDS-RTPS connection to other Modules.
AMM uses the default DDS `domainId` of `0`.
All DDS QoS values are default (per [DDS](https://www.omg.org/spec/DDS/About-DDS/) version 1.4), except as defined in
 comments for each Topic in the [AMM.idl](schema/AMM.idl) specification file.
 
## Module Configuration
Once an AMM Module has established a DDS-RTPS connection to the other Modules on the network, the Module shall
 publish to the `OperationalDescription` and `ModuleConfiguration` Topics, as defined in [AMM.idl](schema/AMM.idl).
 
### `OperationalDescription` Topic
A Module’s Operational Description is static for the Module.
It provides the information required to uniquely identify the Module,
 as well as providing a description of Module functionality via the `capabilities_schema` field.
The `capabilities_schema` field is an XML document describing the Capabilities and Configuration options for the Module.

The `OperationalDescription` Topic is discussed in further detail in the
 [Configuration Data Model](configuration_data_model.md) document.

### `ModuleConfiguration` Topic
This Topic provides the current configuration for a Module, broken down by Capability.
When a scenario is loaded by the core software stack, the Module Manager (or equivalent) publishes messages to the
 `ModuleConfiguration` Topic with the data required for the scenario, per Module.
When a `SAVE` Simulation Control message is published, Modules must also publish an updated `ModuleConfiguration` with
 the appropriate data. Thus, every Module must publish and subscribe to this Topic.
 
The `ModuleConfiguration` Topic is discussed in further detail in the
 [Configuration Data Model](configuration_data_model.md) document.

## Module Operation
Beyond handling configuration, Modules have a couple more behavior requirements: publishing to the `Status` Topic,
 and responding to the `SimulationControl` Topic.
 
### `Status` Topic
Modules shall publish a `Status` update for each Capability that is enabled by configuration.
Modules shall maintain the `value` field to keep it up-to-date in order to best represent the appropriate status of the
 Capability.

The `timestamp` field is updated each time a new Status is published.
The `module_id` field shall match the value of the Module Configuration `module_id` field.
The `educational_encounter` field shall match the value  provided by the configuration that was assigned for the
 current Scenario.

### `SimulationControl` Topic
All Modules shall subscribe to the `SimulationControl` Topic and respond appropriately, as described below.
Modules shall only publish to the `SimulationControl` Topic as a result of direct user input.

#### `RUN`
The `RUN` control indicates that the simulation shall begin or resume, if currently paused.
Prior to the `RUN` control, Modules should not simulate any patient action or movement,
 but may render systemic or initial/current patient (or environmental) state.

#### `HALT`
The `HALT` control indicates that the simulation shall cease progressing in time,
 freezing scenario state, including patient & environmental state.
Modules shall maintain this state until further control is received.

Modules shall also enter this state after receiving updated Configuration, or after receiving a `HALT` control.

#### `RESET`
The `RESET` control indicates that simulation of a given scenario has completely ceased and all Modules shall reset
 their state to default, including resetting their stored `educational_encounter` value to `null`.
Modules shall `HALT` immediately after resetting.

#### `SAVE`
The `SAVE` control indicates that Modules shall publish their current configuration and state data to the appropriate
 `ModuleConfiguration` topic.
This configuration may then be loaded at a later time to ‘reload’ a given scenario from saved state.
Even if a Module has no internal state to save, it shall update the timestamp field of the appropriate
 `ModuleConfiguration` topic to indicate compliance with this control.
