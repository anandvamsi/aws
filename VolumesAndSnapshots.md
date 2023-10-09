# VolumesAndSnapshots

- what is meant by EBS volume
- what are different types of EBS volumes
- Volume state
- How to Create and Attach volume
    - Disk partitioning
- Creating a Snapshot out from a volume
- Create Volume from a snapshot
- AMI
- Copy AMI to Different Regions.


## EBS Volumes
Elastic Block Store (EBS) is a block storage service based in the AWS cloud. EBS stores huge amounts of data in blocks, which work like hard drives (called volumes). 
You can use it to store any type of data, including file systems, transactional data, ```NoSQL and relational databases, backup instances, containers, and application```
### Usecases
- ```Frequent Updates``` - Storage of data that needs frequent updates. For example database application, instances system drive
- ```Throughput-Intensive application``` - that need to perform continous disk scans.

## Classification of EC2
<img src="EBS-classification.PNG" width="600">
