# Prerequisites
You must have the following installed:
* python
* docker
* docker-compose

You should also clone the following repository
* hyperledger-fabric (https://gerrit.hyperledger.org/r/fabric

```sh
$ sudo apt-get install python-setuptools python-dev build-essential
$ sudo easy_install pip
$ sudo pip install behave
$ sudo pip install google
$ sudo pip install protobuf
$ sudo pip install grpc
$ sudo pip install gevent
$ sudo pip install grpcio
$ sudo pip install pyyaml
```

# Environments
This version of the behave tests have been modified such that it can both be executed locally as well as on a remote system.

## Local Network (Default)
By default the behave tests will start and execute a local X-86 network setup. 

### Execute behave tests
There are different options when using behave. Feel free to explore the different options that are available when executing the tests. (http://pythonhosted.org/behave/behave.html)

The most basic execution when executing all of the available tests in the behave/tests/ directory is to simply type:
```sh
$ behave
```


## Executing Behave tests on an Established Remote Network

### Setup
When setting up a "Bring Your Own Network" be sure to include a networkcredentials file in the bddtests directory containing json, that consists of atleast the following information:

```
{
   "PeerData" :  [
   { "name" : "PEER0", "api-host" : "172.17.0.3", "api-port" : "5000" } , 
   { "name" : "PEER1", "api-host" : "172.17.0.4", "api-port" : "5000" } , 
   { "name" : "PEER2", "api-host" : "172.17.0.5", "api-port" : "5000" } , 
   { "name" : "PEER3", "api-host" : "172.17.0.6", "api-port" : "5000" } , 
   { "name" : "PEER4", "api-host" : "172.17.0.7", "api-port" : "5000" } , 
   { "name" : "PEER5", "api-host" : "172.17.0.8", "api-port" : "5000" } , 
   { "name" : "PEER6", "api-host" : "172.17.0.9", "api-port" : "5000" } , 
   { "name" : "PEER7", "api-host" : "172.17.0.10", "api-port" : "5000" } 
   ],
   "PeerGrpc" :  [
   { "api-host" : "172.17.0.3", "api-port" : "30001" } , 
   { "api-host" : "172.17.0.4", "api-port" : "30003" } , 
   { "api-host" : "172.17.0.5", "api-port" : "30005" } , 
   { "api-host" : "172.17.0.6", "api-port" : "30007" } , 
   { "api-host" : "172.17.0.7", "api-port" : "30009" } , 
   { "api-host" : "172.17.0.8", "api-port" : "30011" } , 
   { "api-host" : "172.17.0.9", "api-port" : "30013" } , 
   { "api-host" : "172.17.0.10", "api-port" : "30015" } 
   ],
 "Name": "localpeer_ramesh" 
} 
```

set the SSHPASS environment variable to the password that will be used when ssh-ing to the peer nodes. The default ssh login used is "vagrant", so the password as a default is 'vagrant'.
```sh
$ export SSHPASS=vagrant
```

Be sure to generate ssh keys and distribute them to the peer nodes on the network.
```sh
$ ssh-keygen     # For this test, do not enter a passcode when building this key file. BE SURE TO DELETE/VERIFY DELETION OF THE .ssh DIRECTORY FROM THE PEERS AFTER TESTING.
$ python distribute_keys.py
```
If everything is setup correctly, you should not be prompted for a password as the keys are distributed on the peer nodes.


Execute the python script to setup the network that you would like to have. The script currently sets up 8 different peer nodes.



Execute behave tests
```sh
$ behave --tags=-TLS    # Since we have a set network, we cannot test both https and http network setups. Assuming the remote hosts are using http and not https
```


## Helpful scripts
Generating the networkcredentials file for use in the tests
For zACI environment outside of BlueMix:
```shell
> python update_z.py -f <zACI network file name>
```

For X86 BlueMix environment:
```shell
> python update_z.py -b -f <BlueMix X86 file name>
```


