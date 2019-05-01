I'm trying to move from a single container running gitlab, to 3 containers. One running gitlab, one the postgresql and one the redis container.

https://forum.gitlab.com/t/moving-from-running-ce-in-one-container-to-multiple-containers-on-a-swarm-for-performance-upgrade/26018



The new gitlab system needs to have all the data and files of the current single container gitlab system.



Open up terminal

~~~~
git clone git@github.com:mjdavies/gitlab_test.git
cd gitlab_test
docker swarm init
docker stack deploy -c single-stack.yml git
~~~~

Wait a couple of minutes, you should now have a running gitlab instance.  Go and create a password, a project, and a wiki page at http://localhost/

Stop and remove the stack, and start it again with the multi-stack.yml file

~~~
docker stack rm git
docker stack deploy -c multi-stack.yml git
~~~

Wait a couple of minutes, you should now have a running gitlab instance. Browse to http://localhost

I would expect at this point to be looking at the same database that we setup in the single-stack system, as we're pointing at the same postgresql folder on the local filesystem, but I don't see tha.

Sometimes I see a blank database, and need to create it all again. Sometimes I get erros from the system saying that the tables don't exist.

Can anyone see what I'm doing wrong with the volume mounts? Or is this not possible and I need to do this in a different way, maybe export the database from the single container first, then import it?

