# fleetdm

Testing Fleetdm 

Fleet - https://fleetdm.com/

"Fleet provides a centralized management interface for osquery to deploy, update, and manage osquery agents across a thousand or one hundred thousand devices"

Lab reqs

* Ubuntu server - Docker host for the containers 
* Windows server for testing
* Windows machine with docker engine to generate the MSI 

Note I used my own machine in the lab to create the msi 

All resources were taken from the following post at Redhat [here](https://www.redhat.com/sysadmin/fleetdm-get-started)

On the Ubuntu machine

See guide for install [here](https://docs.docker.com/engine/install/ubuntu/)

Create docker compose file see above

Create folder 

    mkdir fleet

Public key

    openssl ec -in fleet/server.pem \
    -pubout -out fleet/server.pem.pub

 self-signed certificate - I used same as the post fleet.example.com

    openssl req -new -x509 \
    -key fleet/server.pem \
    -out fleet/server.cert -days 365

modify your other host in the lab to have the ip in its host or dns for the ubuntu host and set it to fleet.example.com

execute the following 

    UID=${UID} GID=${GID} docker-compose up

**NOTE:** This is purely for a lab this isnt for production 


Bowse to the webpage and configure 

http://fleet.example.com 


Creating the MSI 

Grab a copy of the [Fleet Binary](https://github.com/fleetdm/fleet/releases)

Extract and add either a path variable to the folder the exe is in or put it in c:\windows\system32

Ensure Docker is running 

Go to the webpage fpr fleet > add hosts > windows and copy and pase the line 

It will look like this

    fleetctl package --type=msi --fleet-url=https://fleet.example.com:8080 --enroll-secret=ABCcdefghijklmno37y28

Execute a container will be created and a msi generated this will be then in your users folder location 

Move the MSI to the windows lab server 

On the windows Lab server run the MSI 
