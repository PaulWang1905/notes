# Docker
**Running Docker as normal user**
  
  By default, Docker is only accessible with root privileges (`sudo`). If you want to use docker as
  a regular user, you need to add your user to the `docker` group.
  
      sudo addgroup --system docker
      sudo adduser $USER docker
      newgrp docker
      sudo snap disable docker
      sudo snap enable docker
  

# Example: Initializing a New Server Directory with a Companion Image.


    sudo docker run --rm -v /home/<username>/RDFox/RDFox.lic:/opt/RDFox/RDFox.lic \
        --mount type=bind,source="$(pwd)",target=/home/rdfox/.RDFox \
        oxfordsemantic/rdfox-init \
        -persistence file \
        -request-logger elf \
        -role admin \
        -password <PASSWORD>
    


# Example: Running in Daemon Mode With Persistence

Using volume rdfox-server-directory, prepared as described in the preceding example, a containerized RDFox daemon, reachable at host port <host-port>, can be launched using the following command:

    docker run -d --cap-drop ALL -p <host-port>:12110 \
               -v /home/<username>/RDFox/RDFox.lic:/opt/RDFox/RDFox.lic \
               --mount type=bind,source="$(pwd)",target=/home/rdfox/.RDFox \
               oxfordsemantic/rdfox


# Shell connection to a remote instance

## remote
```remote``` mode launches a client for the remote shell API to provide command line interation with an RDFox server. The server’s endpoint must be running and reachable from the host running the remote mode process. The syntax for this mode is:

    RDFox [-<remoteShellParameter> <value> ...] [-role <role>] [-password <password>] remote <server-url> [<command> ...]
    
For example: 

    RDFox remote http://example.com:12110

## Anonymous, Read-only REST Access

The following shell example shows how to enable anonymous (unauthenticated), read-only access to the entire server:

    > role create guest
    A new role was created with name "guest".
    > grant privileges read > to guest
    The privilege 'read' over the resource specifier ">" was granted to the role "guest".
    > endpoint start
    The REST endpoint was successfully started at port number/service name 12110 with 11 threads.

With the guest account, the instance will be read only and open to guest. The guest account password is guest and can not be changed. 
On receipt of an anonymous REST request (one with no Authorization header or TLS-authenticated role name), RDFox will attempt to create a new connection for the request by authenticating with role name guest and password guest. To enable anonymous REST access, administrators should therefore create the guest role and grant it the privileges to perform any operation which should be available to any client that can reach the REST endpoint. As a corollary, anonymous REST access can be completely disabled by ensuring the absence of a role named guest from the server’s role list. Note that RDFox will not allow the guest role to be created with any password other than guest, nor will it allow this role’s password to be changed.

