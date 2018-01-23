**AWS Database Week - Day 2 - Non-Relational**

*The New De-Normal: Kitchen Sink Analogy*
Similar to a kitchen sink, there's a lot of different items in the monolith architecture. So you one-by-one remove each item, depending on where it fits. There is then a consistency problem to ensure that there is no missing spoons or utensils. 

*DynamoDB*
Partition Key -> Determines data distribution
Sort Key -> Enables initial sorting

GSI (Globally Secondary Indexes): 
	ALL
	INCLUDE A2 (partition key, table key, a2)
	KEYS_ONLY (partition key, table key)

CreateTable <TableName>
	ParitionKey
	SortKey
	Provisioned Reads (Expected # of reads on this table)
	Provisioned Writes (Expected # of writes on this table)

Provisioned Capacity

Read Capacity Unit (RCU)
	1 RCU returns 4KB of data for strongly consistent reads, or double the data at the same cost for eventually consistent reads

Write Cpacity Unit (WCU)
	1 WCU writes 1KB of data, and each item consumes 1 WCU minimum

Partitioning
Based on the calculated hash will determine which partition it will go to (data entry). 
There could be a partition split due to partition size such that if the partition exceeds capacity, it will automatically create a new partition and split it accordingly. 
There could also be a partition split due to capacity increase.

By design, data is replicated into 3 different AZ's. Such that:
AZ A
	Host A (Partition A)
	Host B (Partition B)
	Host C (Partition C)
AZ B
	Host D (Partition A)
	Host E (Partition B)
	Host F (Partition C)
AZ C 
	Host G (Partition A)
	Host H (Partition B)
	Host I (Partition C)

When a data entry comes in, it will inject into all 3 availability zones.

TTL = Time to Live = Removes data that is no longer relevant
An epoch timestamp marking when a n item can be deleted by a background process, without consuming any provisioned capacity

