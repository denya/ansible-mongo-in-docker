TL;DR
------

Ansible roles to make MongoDB provision in Docker with mounted /mongodata
volume.

* mongo role -- to build & run docker container

* mongo-master role -- to make cluster provision

Mongo role contains workaround for UID/GID mismatch in host system and inside
a container.

Check task & templates.

In host system we create new user with predefined UID.
We have own Dockerfile derived from the official 'mongo' that includes
change_uid.sh to change UID/GID inside image.


Long one
---------

https://github.com/denya/ansible-mongo-in-docker

hey, here is two ansible roles for mongodb provision.
general role is  ‘mongo’

https://github.com/denya/ansible-mongo-in-docker/blob/master/mongo/tasks/mongo-init.yml

important thing here, that I build own mongodb image with small fix for user
rights

https://github.com/denya/ansible-mongo-in-docker/tree/master/mongo/templates/mongo-oem-docker

the problem is that when you use docker volumes to mount host folder with
mongodata to docker container, you should have mongo user in both host system
and container with the same uid/gid!

So important step in this ansible task is — create user/group with predefined
uid in host system, then generate change_uid.sh with this uid/gid. Then build
own mongodb container derived from original one & then launch containers

:slightly_smiling_face:


------------------


role for mongo-master — that make provision of the cluster:

https://github.com/denya/ansible-mongo-in-docker/blob/master/mongo-master/tasks/mongo-master-init.yml

so it’s just render 3 js scripts and then run it
template of script for ReplicaSet initialisation is here:

https://github.com/denya/ansible-mongo-in-docker/blob/master/mongo-master/templates/repset_init.j2
