# Block Storage \(Volumes\)

Block storage volumes are network-based block devices that provide additional data storage for Cloud Servers. You can move them between Cloud Servers and resize them at any time.

Volumes are most useful when you need more storage space but don’t need the additional processing power or memory that a larger Cloud Server would provide, like:

* As the document root or media upload directory for a web server
* To house database files for a database server
* As a target location for backups
* As expanded storage for personal file hosting platforms like ownCloud
* As components for building more advanced storage solutions, like RAID arrays

Volumes work as generic block devices, so you can treat attached volumes like locally connected storage drives. This lets you partition, format, and manage volumes with familiar tools and techniques.

Volumes are independent resources, so you can move them between Cloud Servers in the same datacenter, and you can increase the size of a volume without powering down the Cloud Server it’s attached to.

Volumes are encrypted and store data on hardware that is separated from the Cloud Server and replicated multiple times across different racks, reducing the chances of data loss because of hardware failure.

## Plans and Pricing

Volumes cost € 0.05 / GB + VAT per GiB per month and range from 10 GB to 2 TB \(2,000 GB\). Charges accrue hourly for as long as the volume exists.





