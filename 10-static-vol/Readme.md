## Access Modes / Mount Options

https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes


### ReadOnlyMany 
	External Storage can be mounted in ReadOnly on all nodes.

### ReadWriteOnce 
	External Storage can be mounted in RW mode only	on single node.

### ReadWriteMany 
	 External Storage can be mounted in ReadWrite on all nodes.


RWO ---> Azure Disk, AWS EBS
RWX ---> Azure FileShare, NFS
ROX ---> Azure FileShare

---

Static Provisioning		: Storage Service already initialized, capacity allocation done
It may even have old data.	
									
Deleting volume will NEVER erase data from storage.				
	
		
Dynamic Provisioning   : Storage Service ready but capacity allocation not done!
Kubernetes would use "an extension" [ storage provisioner]
									
> You must set "retention policy" as one of below

RETAIN :> Keep the data.

RECLAIM :> Erase data but keep the storage unit. Allows reuse.

DELETE  :> Erase data and storage unit.