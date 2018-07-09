Assignment for Garth

Roles created for the workflow

app-servers:
Role to install and enable tomcat
Copies over index.html template and has a handler to restart tomcat service

aws_smoketest:
Role to smoke test the aws 3tier app

openstack_clean:
Role to delete the openstack instances

openstack_smoketest:
Role to smoke test the openstack 3tier app

openstack_facts:
Gathers openstack facts with the os_server_facts module
Adds hosts to in-memory inventory with their group and deployment name metadata

prep-servers: 
This role copies the repo file over and grabs the secure url from vaulted var.
Installs common packages from a variable list
Enables sudo without tty to resolve an issue with installing postgres

tower-aws-credential:
This role installs tower-cli and pre-req packages onto the AWS bastion host, it then finds the ssh private key for that environment and creates a tower credential (overwrites/updates existing one with new priv key) and then uninstalls tower-cli

Other Roles used in workflow

aws_provision:
Role created by instructor to order AWS instance

geerlingguy.postgresql:
Community role to install postgres

geerlingguy.haproxy:
Community role to install haproxy, modified the haproxy config template to loop through the inventory to get all the app servers and deal with the lack of GUID on the openstack servers.


Other roles used elsewhere
openstack_flavor:
Role to create the openstack flavor

openstack_key:
Role to create ssh keypair and add to openstack instances

openstack_network:
Role to create the openstack networks

osp-securitygroup:
Instructor role used to create the openstack security groups




Issues faced:
HAproxy config on the frontend needed a way to add app servers that didnt have a GUID, so the config template was changed to loop through the inventory instead. That way it would add all the app servers in the 3tier app for both the openstack and aws environments.

Install postgres with the community role via tower threw an error due to no tty and sudo, had to modify sudoers to allow sudo without tty 

Openstack environment takes a long time to install and come online, I had to put in enough retries to give the networking time to come online for the smoketest to have a chance to pass.

Each time the 'prod' aws environment is provisioned it has a new private key, so eventually came to the conclusion the easiest method to retrieve it/create a tower credential was to temporarily install tower-cli on the bastion host to create/update the ec2-user credential.






