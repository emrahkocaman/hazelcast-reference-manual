
### WAN Replication Queue Full Behavior

Before 3.6 release, WAN replication drops the new events if WAN replication event queues are full.
Starting with 3.6, this behavior is configurable. There two different supported behaviors.
 
- `DISCARD_AFTER_MUTATION`: When this option is selected, new WAN events generated by the member will be dropped and will not be replicated to target cluster
when the WAN event queues are full.   
- `THROW_EXCEPTION`: When this option is selected, WAN queue size will be checked before each supported mutating operation (like `IMap#put`, `ICache#put`)
and if one the queues of target clusters is full, `WANReplicationQueueFullException` will be thrown and operation won't be allowed.

The following is an example configuration:

```xml
<wan-replication name="my-wan-cluster">
  <target-cluster group-name="test-cluster-1" group-password="test-pass">
    ...
    <queue-full-behavior>ACK_ON_OPERATION_COMPLETE</queue-full-behavior>
  </target-cluster>
</wan-replication>
```

![image](images/NoteSmall.jpg) ***NOTE:*** *`queue-full-behavior` configuration is optional. Default value of it is `DISCARD_AFTER_MUTATION`*.


