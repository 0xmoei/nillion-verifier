# Update Nillion Verifier

## Docker container lists
Copy Container ID of `nillion/verifier`
```console
docker ps
```

## Kill Current Verifier
Replace `CONTAINER_ID`
```
docker stop CONTAINER_ID
docker rm CONTAINER_ID
```

## Transfer Accuser firectory contents to Verifier contents
```
cd $HOME
mkdir -p nillion/verifier
cp -r nillion/accuser/* nillion/verifier/
```

## Start Verifier Again
```bash
docker run -d -v ./nillion/verifier:/var/tmp nillion/verifier:v1.0.1 verify --rpc-endpoint "https://testnet-nillion-rpc.lavenderfive.com"
```
> Now check your logs

Once your verifier is updated, it enters a 10 minutes warmup. If the network doesn’t require verification at that moment, your verifier’s status will show “Verifying: False.” Once needed, your verifier’s status will show “Verifying: true” and the verifier will send challenges to Nilchain.
