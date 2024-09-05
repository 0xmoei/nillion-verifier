# Nillion Verifier Node

### Official dashboard
https://verifier.nillion.com/verifier

## Faucet
Create a Nillion address in your [keplr](https://chromewebstore.google.com/detail/keplr/dmkamcknogkgcdfhhbddcghachkejeap) wallet and get faucet for it
https://faucet.testnet.nillion.com/

## Install docker
```
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Docker version check
docker --version
```

## Install verifier
1- Pull docker images
```
docker pull nillion/retailtoken-accuser:latest
```

2- Create folder
```
mkdir -p nillion/accuser
```

3- Initialize verifier
```
docker run -v ./nillion/accuser:/var/tmp nillion/retailtoken-accuser:latest initialise
```
This will output the details needed to register the accuser on the website, save them:
* accound_id: Nillion address of the accuser
* public_key: Public Key of the accuser
* Note The accuser will store the credentials in a file called `credentials.json` in the folder that was created. If you lose this file, you will lose access to the keys/address of the accuser


4- Check `credential.json` file contents
```
cat nillion/accuser/credentials.json
```

5- Send some $NIL to verifier accuser address or get [faucet](https://faucet.testnet.nillion.com/) for it

## Run verifier accuser
```
docker run -d -v ./nillion/accuser:/var/tmp nillion/retailtoken-accuser:latest accuse --rpc-endpoint "https://nillion-testnet.rpc.kjnodes.com" --block-start 5309742
```


## Verify your node in the dashboard
1- Head to [verifier dashboard](https://verifier.nillion.com/verifier)

2- Click **Linux setup** to open Verifier installation docs, add your node credentials in step 5 to verify your node

![Screenshot_240](https://github.com/user-attachments/assets/704e0da9-be72-4475-8d29-14ca821fe575)

![Screenshot_239](https://github.com/user-attachments/assets/486ef218-32b1-4547-8015-7fbc59b5ef17)

## Check Node status
**1- Docker container lists**
```
docker ps
```
* Copy Container ID of your `nillion/retailtoken-accuser:latest` container


**2- Check your latest verified secrets, Replace `CONTAINER_ID` with yours**
```
docker logs -f -n 10 CONTAINER_ID
```
* the amount must inscreases
* Registered must be `true`

![Screenshot_241](https://github.com/user-attachments/assets/7121acd6-04ae-445a-b1e5-15fdb63fb6f7)



## Usfeull commands: if you get into any error during the verifier running

Restart Container
```
docker restart CONTAINER_ID
```

Stop & remove Container
```
docker stop CONTAINER_ID && docker rm CONTAINER_ID
```

Change RPC (after stop and remove old verifier)
```
docker run -d -v ./nillion/accuser:/var/tmp nillion/retailtoken-accuser:latest accuse --rpc-endpoint "https://nillion-testnet-rpc.polkachu.com/" --block-start 5309742
```


