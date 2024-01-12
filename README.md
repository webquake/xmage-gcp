# XMage server on Google Cloud

Based on the [DigitalOcean Tutorial](https://github.com/goesta/docker-xmage-alpine/wiki/DigitalOcean-Tutorial) by [goesta](https://github.com/goesta), I created this little guide on how to run an xmage server on Google Cloud.

1. Login to your Google Cloud account or sign up with free credits (as of 2024, $300 for the first 90 days):
   
https://cloud.google.com/free

Please note, a credit card (not a pre-paid debit card) is required.

2. Search for "firewall" and click "Create Firewall Rule" with the following settings:
* Name: `allow-incoming-traffic`
* Target tags: `xmage`
* Source IPv4 ranges: `0.0.0.0/0`
* Specified protocols and ports: TCP > Ports: `17171,17179`

3. Go to "Compute Engine". (If this is a new project, you have to "enable" the API first.)

4. Click "Create Instance" with the following settings:
* Boot disk > Operating System: "Container Optimized OS"
* Advanced Options > Networking > Network tags: `xmage`

5. Note the external IP address.

6. SSH into the newly created VM and "Authorize".

7. Replace both occurrences of "ABC.DEF.GHI.JKL" with the external IP address of your VM and then run this command:

```
sudo docker run --rm -it \
    -p 17171:17171 \
    -p 17179:17179 \
    --add-host ABC.DEF.GHI.JKL.nip.io:0.0.0.0 \
    -e "XMAGE_DOCKER_SERVER_ADDRESS=ABC.DEF.GHI.JKL.nip.io" \
    goesta/xmage-beta
```

8. Connect to server from an XMage client
* Server name: `ABC.DEF.GHI.JKL`
* Port: `17171`

9. Make sure to "delete" (not just "stop") the VM after use to avoid paying anything while the server is offline.


## Troubleshooting:

* Ensure that the VM has an external IP address:

Advanced Options > Networking > Network interfaces > "default" (or name of your network) > External IPv4 address > "Ephemeral"

* Unable to connect from XMage client:

Review the firewall setting to allow incoming traffic.
