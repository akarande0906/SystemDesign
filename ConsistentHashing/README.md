# Consistent Hashing with LRU Cache

This simple project implements Consistent Hashing with one or more nodes (and associated virtual nodes). Each Virtual node is an instance of an LRU Cache (Least Recently used).

I'm jumping the gun here. Here's a quick dump on Consistent Hashing and LRU Caches.

## Consistent Hashing

Consistent Hashing is an algorithm/technique that efficiently redestributes data when adding or removing an instance in a distributed system. 

We could use a simple module function to assign keys to a specific cache instance using the formula: $hash(key) % N $(where N is the number of instances). This works perfectly as long as we don't add/remove any instances from the distributed setup. Adding/Removing instances will result in a reallocation of most keys across instances, which is a costly overhead. 

![alt text](images/SimpleHash.jpg)

Consistent Hashing overcomes this issue by using a hash ring, a circular ring where instances are placed based on a hash function. When a key is to be allotted to an instance on the ring, the hash of the key is computed (using the same hash function as used for the instances) and we find the nearest instance based on the hash going clockwise (or the next nearest node with a hash key greater than the key's hash). If the hash of the key is greater than the last node on the ring, we allocate the key to the first node on the ring (hence the ring). 

When a new instance is added or removed, we simply place the instance on the ring and only the keys that were previously allotted to a nearby instance, but belong to the new instance based on its placement are reallotted to the new instance. None of the other instances are affected.

![alt text](images/HashingRing.jpg)

There is excdllent material on the web that explains Consistent Hashing in great detail.


## LRU Cache

Here we use a common type of Cache called the Least-recently-used Cache. The Least-recently-used (or LRU) specifies the eviction policy used to remove items when the the cache capacity is reached. In this type of cache, in the event that the capacity is reached and space needs to be made for new items, we remove the item that was least recently accessed. Access is determined based on how recently an item on the cache was added/updated or read. 

![alt text](images/LRUCache.jpg)

## Setup

### Prerequisites

**Note:** This setup is specifically written for and tested on a MAC, but will work with appropriate updates for Windows/Linux as well. I will leave that to you. 

## 1. Install Docker

I use docker to deploy multiple instances of the LRU cache and manage them through consistent hashing. 
On MAC, install Docker Desktop using this link: https://docs.docker.com/desktop/setup/install/mac-install/

Once installed, run the Docker Desktop app.

## 2. Install python3 and necessary packages

This code is written in python. Download and install the latest stable version of python3.
Install the following packages using pip3:

*docker, 
flask*

## 3. Update Environmental variables

The following environmental variables should be specified as required:

**CACHE_SIZE**: This sets the size of the Cache for each of the Cache Nodes. 

***SERVERS***: Comma separated list of servers that are part of the consistent hash ring.

***REPLICATION_FACTOR***: The replication factor determines how many virtual nodes are to be added. Specifying a value of 2 would mean one Server instance + one Virtual node instance. Virtual modes are used to uniformly distribute the load on one server. 


## 4. Testing

### Option 1: Local Python testing

### Option 2: Testing wih docker containerized Cache nodes



