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

## Start Verifier Again
```bash
docker run -d -v ./nillion/accuser:/var/tmp nillion/verifier:v1.0.0 verify --rpc-endpoint "https://testnet-nillion-rpc.lavenderfive.com"
```

Once your verifier is updated, it enters a 3600-second (1 hour) “sleeping” period. After this, the verifier will sync with the Nillion Network and periodically check if it is needed to scan new blocks and verify secrets. If the network doesn’t require verification at that moment, your verifier’s status will show “Verifying: False.” Once needed, your verifier’s status will show “Verifying: true” and the verifier will send challenges to Nilchain.
