# Specification of the Advanced Modular Manikin (AMM)
This repository documents the data and behavior requirements of the AMM interoperability specification.

An AMM system is logically – and frequently, physically – divided into a collection of independent Modules.
A Module is, at minimum, a computer running code that communicates via the AMM communication interoperability
 specification, described here.
Modules that simulate a physical segment of the human body have additional physical requirements.
 
In order for any Module to comply with AMM, it must communicate as described by the
 [Operational Data Model](operational_data_model.md),
 [Configuration Data Model](configuration_data_model.md), and 
 [Module Behavior Requirements](module_behavior_requirements.md) documents.
 
 Educational Encounter Scenarios may be defined using the [Scenario File specification](scenario_file_specification.md).
