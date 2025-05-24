What is WATCloud?
- A student group w/ a goal: make powerful computation fair and accessible to everyone.
- Largest student-ran server cluster in Canada
	- 500+ users from different Unis
	- 6 servers: SLURM & regular usage (SSH + bash) nodes

What did I do on WatCloud?
- Configuring the hardware; adding SWAP memory
- Working on the automatic provisioning sytem:
	- We have a web app for users to edit profile
	- Its done through YAML to configure the forms, then the data gets automatically provisioned by ansible / terraform
- Created a cronjob deployed on K8s to periodically save assets to a self-hosted Ceph S3 bucket

#### SWAP
- Some extra RAM space thats stored in the hard drive
	- Prevents crashes: critical OS processes could still function on the SWAP memory, isntead of instantly crashing
	- Handles Memory Spikes: apps w/ occasional high mem use can use this
	- Supports hibernation: can save system memory onto swap while RAM is cleared
- Did this w/ Ansible provisioning system
- https://github.com/WATonomous/infra-config/pull/2297/files
#### Provisioning System
- We use Terraform for Infra as Code
- Ansible to configure the bare metal machines
- We have YAML files for individual users -> modify yaml files = user perms modified
#### Kubernetes
- We have our own kubernetes cluster hosted on one of the servers
- A cronjob spins up once a day to save assets into corresponding S3 Ceph Bucket
	- Work flow is this: dev adds upload asset to an URL + gets a return link
	- Can use the return link in the website code
	- Upon building, website displays the image stored at the URL (inside bucket)
	- Once their PR is merged, the asset is moved from temp -> perm bucket

#### S3 Ceph Buckets
- Ceph is open source, distributed storage system -> high scalability & maintainability. 
- We use object buckets, very similar to Amazon S3 & same API
- Useful because we get versioning, access control (only that cronjob can upload / download files)