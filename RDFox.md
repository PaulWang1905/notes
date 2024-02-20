# Docker
**Running Docker as normal user**
  
  By default, Docker is only accessible with root privileges (`sudo`). If you want to use docker as
  a regular user, you need to add your user to the `docker` group.
  
      sudo addgroup --system docker
      sudo adduser $USER docker
      newgrp docker
      sudo snap disable docker
      sudo snap enable docker
  

# RDFox Example: Running Interactively With Persistence


    sudo docker run --rm -v /home/<username>/RDFox/RDFox.lic:/opt/RDFox/RDFox.lic \
        --mount type=bind,source="$(pwd)",target=/home/rdfox/.RDFox \
        oxfordsemantic/rdfox-init 
        -persistence file \
        -request-logger elf \
        -role admin \
        -password \
    
