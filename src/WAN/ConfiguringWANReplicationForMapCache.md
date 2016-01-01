

### Configuring WAN Replication for IMap and ICache

Yon can configure the WAN replication for Hazelcast's IMap and ICache data structures. To enable WAN replication for an IMap or ICache instance, you can use `wan-replication-ref` element. Each IMap and ICache instance can have different WAN replication configurations.

**Enabling WAN Replication for IMap:**

Imagine you have different distributed maps, however only one map should be replicated to a target cluster. To achieve this, configure the map to be
replicated by adding the `wan-replication-ref` element in the map configuration as shown below.

```xml
<hazelcast>
  <wan-replication name="my-wan-cluster">
    ...
  </wan-replication>
  <map name="my-shared-map">
    <wan-replication-ref name="my-wan-cluster">
       <merge-policy>com.hazelcast.map.merge.PassThroughMergePolicy</merge-policy>
      ...
    </wan-replication-ref>
  </map>
  ...
</hazelcast>
```
You see that we have `my-shared-map` configured to replicate itself to the cluster targets defined in the earlier
`wan-replication` element.

When using Active-Active Replication, multiple clusters can simultaneously update the same entry in a distributed data structure.
You can configure a merge policy to resolve these potential conflicts, as shown in the above example configuration (using the `merge-policy` sub-element under the `wan-replication-ref` element).

Hazelcast provides the following merge policies for IMap:

- `com.hazelcast.map.merge.PutIfAbsentMapMergePolicy`: Incoming entry merges from the source map to the target map if it does not exist in the target map.
- `com.hazelcast.map.merge.HigherHitsMapMergePolicy`: Incoming entry merges from the source map to the target map if the source entry has more hits than the target one.
- `com.hazelcast.map.merge.PassThroughMergePolicy`: Incoming entry merges from the source map to the target map unless the incoming entry is not null.
- `com.hazelcast.map.merge.LatestUpdateMapMergePolicy`: Incoming entry merges from the source map to the target map if the source entry has been updated more recently than the target entry. Please note that this merge policy can only be used when the clusters' clocks are in sync.

![image](images/NoteSmall.jpg) ***NOTE:*** *When using WAN replication, please note that only key based events are replicated to the target cluster. Operations like `clear`, `destroy` and `evictAll` are NOT replicated.*

<br></br>

**Enabling WAN Replication for ICache:**



Hazelcast provides the following merge policies for IMap:

- `com.hazelcast.cache.merge.HigherHitsCacheMergePolicy`: Incoming entry merges from the source cache to the target cache if the source entry has more hits than the target one.
- `com.hazelcast.cache.merge.PassThroughCacheMergePolicy`: Incoming entry merges from the source cache to the target cache unless the incoming entry is not null.