# rights
We will use the standard /srv folder for shared data

## list groups and owners
```bash
getent group
```
Will give an overview of all users and groups.
```bash
ls -ls /srv/shared
```
will show the group that has rights to that folder if it exists.

## create group and give rights
For a new server, you can do
```bash
sudo groupadd collaborators
sudo mkdir -p /srv/shared
sudo chgrp collaborators /srv/shared
sudo chmod g+rws,o-w /srv/shared
sudo usermod -aG collaborators another_user
```
This creates the group collaborators and give them rights to the srv/shared folder
chmod gives the proper rights to the shared folder 
- g+rws: Gives the group read, write, and execute (setgid) permissions.
- o-w: Removes write permission for others (everyone else) for security.

The last command adds `another_user` to the group, in addition to the current user. Replace with the desired user you want to add.

# installation
## install docker
```bash
curl -sSL https://raw.githubusercontent.com/raoulg/serverinstall/refs/heads/master/install-docker.sh | bash
```
This is a script that will properly install docker and dockercompose on the server

# kick off traefik 

## create a hashed password
```bash
sudo apt install apache2-utils
htpasswd -nb admin mySuperSecretPassword
```

This password can be added to the `traefik_dynamic.yml` file, it is a password to access the traefik dashboard.

```traefik_dynamic.yml
  middlewares:
    # Define a middleware for basic authentication
    auth:
      basicAuth:
        # Replace this with the output from your htpasswd command
        users:
          - "admin:$apr1$b5T0G7eG$0Y1/lEX8Uv4D3Kzsa9z2T/"
```


## docker-compose.yml
docker compose runs the traefik image, and it includes other files, eg ldviewer.yml

## start the service
you can start with
```bash
docker compose -f docker-compose.yml -f ldviewer.yml up -d
```

# surfcontroller
This doesnt need traefik for now.
It can run independent with 
```bash
docker compose -f surfcontroller.yml up -d
```



