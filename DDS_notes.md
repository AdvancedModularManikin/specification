#### Design considerations:
All data within a Topic shares the same QoS.
This likely means not all physiology data can live in one Topic.
Thus, it probably makes the most sense to have many, very specialized topics.

#### Useful sections in the [DDS Specification](http://www.omg.org/spec/DDS/1.4):

###### 2.2.2.5.1.9 DDS Data Access Patterns
Good high-level breakdown of how to use DDS.

###### 2.2.4.3.2 Listener access to Read Communication Status
More details on how to actually read data from DDS when it's updated.

###### 2.2.4.4 Conditions and Wait-sets
An alternative architecture on how to handle data updates with a much more thorough discusion of this access pattern.

###### 2.2.5 Built-in Topics
This describes how DDS provides info about every Topic and every reader & writer connected to a domain.
They could be very useful for monitoring & logging.

###### 2.2.6 Interaction Model
Super useful ladder diagrams depicting how DDS is designed to actually be used.
These would've been very useful to see much earlier in the document.
