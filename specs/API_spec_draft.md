
```
- reserve($IYO_UID,$reservationId, $reservationAdminSecret,$size,$expirationEpoch=0)
	- makes a reservation for amount of storage in this NOS
	- $reservationId is unique id (upto 16 chars) which identifies the reservation
	- $size is a number of megabytes to reserve
	- $expirationEpoch - all data removed if 

- unreserve($IYO_UID,$reservationId,$reservationAdminSecret)
	- BE CAREFULL DATA WILL BE REMOVED

- put($IYO_UID,$dataSecret,$reservationId,$key,$data,$consumers=[])
	- $dataSecret required to be able to write into the reservation
	- will store data with max size 1MB
	- data is stored using format defined above
	- crc will be calculated
	- if $consumers not in consumers bytes then add
	- if more than 10 consumers then $byte1=255 $bytes2="" \
		- this means object is permanent, cannot be deleted
	- if object marked as permanent then don't add consumers
	- if object marked for deletion $byte1=254, add consumer and active obj again
	- if key=="" then calculate md5 or blake32 hash (TODO:)
		
- puts($IYO_UID,$dataSecret,$reservationId,[$keys],[$data],$consumers=[])
	- same logic as put but for more objects at once
	
- get($IYO_UID,$dataSecret,$reservationId,$key,verify=True)
	- check CRC if verify == True, if wrong return
	- check if object is in deletion mode, if yes return ""
	- if it doesn't exist return "" & state NOTEXIST
	- return $data
	- if not exist return ???
		
- gets($IYO_UID,$dataSecret,$reservationId,[$key],verify=True)
	- return as list
	- if one does not exist return ???
	
- list(($IYO_UID,$dataSecret,$reservationId)
	- list of keys in the reservation 
	
- delete($IYO_UID,$dataSecret,$reservationId,$key)
	- if not exist do nothing
	- if exists remove the consumer from the $consumers field
	- if exists & its the only consumer then mark object for deletion (do not remove!)
	
- deletes($IYO_UID,$dataSecret,$reservationId,[$key])

- exist($IYO_UID,$dataSecret,$reservationId,$key)

- exists($IYO_UID,$dataSecret,$reservationId,[$key,...])

- existsMark($IYO_UID,$dataSecret,$reservationId,[$key,...],$consumers=[])
	- return [$keyOfNonExistingObj, ...] return the ones which did not exist yet
	- this is to be used if someone wants to upload a bulk of objects

- info($IYO_UID,$dataSecret,$reservationId)
	- returns json of amount of storage used / expirationc/acl
	- only for admin of a reservation

- aclSet($IYO_UID,$adminSecret,$reservationId,$dataSecret,$right)
	- only an reservation admin itself can call this method
	- updates the acl
	- $right is RWDA . (Read Write Delete Admin)	

- actDelete($IYO_UID,$adminSecret,$reservationId,$dataSecret)
	- removes the acl info for that $dataSecret

- stats($IYO_UID,$adminSecret,$reservationId)
	- returns json with
		- nr of requests per hour
		- nr of objects stored
		- size of the data
	- only reservation admin can call this

```
