docker_hadoop
=============

hadoop image build on ubuntu,fully support single, pseudo, and clustering mode

first,you need to have docker installed, then read on...

clustering mode
---------------
docker pull notyy/docker_hadoop:2.4.1_cluster

** start namenode in interactive mode **

in interactive, you start in bash inside of the namenode container, so that you can read logs, do some dfs operation,etc
docker run --name=namenode -ti -h=namenode -p 50070:50070 -p 8088:8088 -p 19888:19888 notyy/docker_hadoop:2.4.1_cluster /etc/bootstrap.sh -bashn

** start namenode in daemon mode **

docker run --name=namenode -d -h=namenode -p 50070:50070 -p 8088:8088 -p 19888:19888 notyy/docker_hadoop:2.4.1_cluster /etc/bootstrap.sh -nd

this will run daemon mode, but since I don't have ssh installed in container, you won't be able to look inside.

-------------------

no matter in which mode, you can open browser, point to http://localhost:50070 to see namenode web UI, and you should notice, in datanode tab, that
there is currently 0 datanode connected

 ** note if you are running docker on a mac,or windows, the containers ports are bind to a virtualbox vm on your
 host os, so http://localhost:50070 won't work, you should run "boot2docker ip" to see VM's ip, then point to http://VM_IP:50070 ,
 replace VM_IP with the actual ip you found out **

 ** now start some datanode **
 ** if you are in active mode of namenode, DON'T QUIT **

 start another terminal and run:

 docker run --name=datanode1 -d -h=datanode1 --link namenode:namenode notyy/docker_hadoop:2.4.1_cluster /etc/bootstrap.sh -dd
 docker run --name=datanode2 -d -h=datanode2 --link namenode:namenode notyy/docker_hadoop:2.4.1_cluster /etc/bootstrap.sh -dd
 docker run --name=datanode3 -d -h=datanode3 --link namenode:namenode notyy/docker_hadoop:2.4.1_cluster /etc/bootstrap.sh -dd

 you can run as many datanodes as you want(and your machine support), remember to modify --name and -h

 now goto 50070 port, you should see you newly added datanodes are working,enjoy!
 =================================================================================
