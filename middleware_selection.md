## Background
Our proof-of-concept prototype featured an extremely simple message broker and ASCII message protocol.
We chose that architecture for expediency of development due to our limited timeline.
While we recognized that the software and protocol we developed was unsuited for broader development, it did allow us the opportunity to explore design decisions which proved to be very beneficial.
Key among these was having a fixed data format, but not restricting content types or values.
This enabled us to employ a flexible publish/subscribe message paradigm, which greatly simplified module development.

## Requirements
Due to our prior explorations, we knew we wanted to leverage the advantages of a pub/sub architecture with extremely flexible content and metadata.
Additionally, due to shortcomings of our earlier implementation, we knew we needed to manage module connectivity, discovery, and message delivery performance, so ease of implementation of these features was important.
Data throughput of the message broker was another concern, since the physiology engine is likely to be generating significant amounts of data that needs to be delivered to many different modules.
Lastly, because many modules will be running on lower-powered embedded hardware, the middleware libraries must be sufficiently light-weight. However, due to developments in ARM hardware, this last consideration became mostly moot.

## Message Queues
We looked at several different MQs, with ZeroMQ and MQTT both offering much of what we desired.

## DDS
> [I]f a lasting architecture is a key benefit, consider data-centric infrastructure. It will require more upfront planning. The concepts are likely less familiar, and will therefore seem complex. But, benefits do accrue as the system incorporates more applications, scales, and evolves over time. Distributed teams, complex system integration, and long-term viability easily justify the added initial investment in data centricity.
[Message-centric vs Data-centric Middleware](http://electronicdesign.com/embedded/whats-difference-between-message-centric-and-data-centric-middleware)


[Messaging Technologies for the Industrial Internet and the Internet of Things Whitepaper](http://www.prismtech.com/download-documents/1561)