Starting from [BioGears CDM](https://biogearsengine.com/documentation/_c_d_m.html), with variable [Tables](https://www.biogearsengine.com/documentation/_c_d_m_tables.html)

All variables in the Systems tables will be published every simulated 'tick'.
For performance reasons, this data will be delivered via 'best effort' QoS settings.
These variables need to be received by modules as a 'unit' so that data stagnation doesn't occur, where some variables are much older than others.
DDS enables this via 'coherent access'.
Due to limitations in RTPS, coherent access is limited to a single DDS Topic.

Module creators will need to include the IDL file specifying this Systems data, and then create IDL for the filtered data type needed by the module.

Electro Cardio Gram Interpolation Waveform will need to be its own topic, published every 'tick', but with reliable QoS settings.

