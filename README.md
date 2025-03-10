# Multi-threaded Key-Value Store (2 phase-commit protocol)

## Steps to run Server through Terminal -
 1. Open terminal in the src folder.
 2. Use javac command to compile the files - `javac server/*.java`
 3. Use the command to specify the port number and a server name and run the server.- `java server/ServerImpl <port-number> <server-name>` ( A sample command  - `java server/ServerImpl 5002 test-server`)
 4. This starts 5 replica servers. If you started the server on port 5002 then the 5 replica servers will be started on ports 5003,5004,5005,5006 and 5007. 
 5. Each replica server has its own key value store which stores a key of type string and value also of type string.
 6. The Servers then awaits the client connection.

## Steps to close Server through Terminal -
1. Press `Ctrl + C`

## Steps to run Client through Terminal -
1. Open terminal in the src folder.
2. Use javac command to compile the files if not already compiled before - `javac server/*.java`
3. Use the command to specify the port number of any one of the replica servers, hostname of the server, a client name and the server name to which you are connecting
(the server name should be the same that you specified while starting the server) and run the Client - `java server/Client <Hostname> <port-number> <server-name> <client-name>`
(A sample command - `java server/Client localhost 5003 test-server test-client-1`)
4. The Client starts and connects to the replica server on the port specified. The client then pre-populates the key value store using the data-population-script.txt. 
5. The user needs to input `run` to run the 15 GET, PUT and DELETE Operations through a script, or input `console` to run the commands manually. The operations are run using the `operations-script.txt`. The user can change the commands in this script file if they want to run any other operations.


## Steps to close Client through Terminal -
1. The user can input `close` to exit.

## Steps to run Server & Client on Docker -
1. Create a docker network if not already created using the command - `docker network create my-network`
2. Create docker image for the server - open the terminal in the src folder and run the command in the terminal - `docker build -t <specify-a-server-image-name> -f Dockerfile.server .`
   <br/><br/> **(Sample command - `docker build -t test-server-image -f Dockerfile.server .`)** <br/><br/> 
3. Run the docker image of the server using the command - `docker run -d --network my-network --name <specify-a-server-container-name> -p <localport>:<docker-port> <server-image-name-specified> <server-port-number> <server-name>` 
<br/><br/> **(Sample command - `docker run -d --network my-network --name test-server-container -p 1009:1009 test-server-image
   5002 test-server`)** <br/><br/>
4. Create docker image for the client - open the terminal in src folder and run the command in the terminal -`docker build -t <specify-a-client-image-name> -f Dockerfile.client .`
<br/><br/> **(Sample command - `docker build -t test-client-image-1 -f Dockerfile.client .`)** <br/><br/>
5. Now run the following command to get the IP of the server container - `SERVER_IP=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <name-specified-for-the-server-container>)`  
<br/><br/> **(Sample Command - `SERVER_IP=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' test-server-container)`)** <br/><br/>
7. Run the client docker image - `docker run -it --network my-network --name <specify-a-client-container-name> <client-image-name> $SERVER_IP <specify-replica-server-port> <server-name> <client-name>`
   <br/><br/> **(Sample Command - `docker run -it --network my-network --name test-client-container-1 test-client-image-1 $SERVER_IP 5004 test-server test-client-1` )** <br/><br/>
8. To run more client containers, run line 5 in a new terminal in the src folder and then use the sample commands below - <br/><br/> 
This client connects to server on port 5005 - `docker run -it --network my-network --name test-client-container-2 test-client-image-1 $SERVER_IP 5005 test-server test-client-2` <br/><br/>
This client connects to server on port 5006 - `docker run -it --network my-network --name test-client-container-3 test-client-image-1 $SERVER_IP 5006 test-server test-client-3` <br/><br/>
9. For the client - The user needs to input `run` to run the 15 GET, PUT and DELETE Operations through a script, or input `console` to run the commands manually. The operations are run using the `operations-script.txt` in the res folder. The user can change the commands in this script file if they want to run any other operations.
10. To exit the client type `close`. 


