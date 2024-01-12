Based on the [DigitalOcean Tutorial](https://github.com/goesta/docker-xmage-alpine/wiki/DigitalOcean-Tutorial) by [goesta](https://github.com/goesta), I created this little guide on how to run an [XMage](http://xmage.today/) server on Google Cloud.

__1. Login to your Google Cloud account or sign up with free credits:__ 

https://cloud.google.com/free

As of 2024, you get $300 of credits for the first 90 days. Please note, a credit card (not a pre-paid debit card) is required.

__2. Search for "firewall" and click "Create Firewall Rule" with the following settings:__
* Name: `allow-incoming-traffic`
* Target tags: `xmage`
* Source IPv4 ranges: `0.0.0.0/0`
* Specified protocols and ports: TCP > Ports: `17171,17179`

__3. Go to "Compute Engine" and click "Create Instance" with the following settings:__ 

(If this is a new project, you might have to "enable" the API first.)

* Boot disk > Operating System: "Container Optimized OS"
* Advanced Options > Networking > Network tags: `xmage`

__4. Note the external IP address.__

__5. SSH into the newly created VM and "Authorize".__

__6. Replace both occurrences of "ABC.DEF.GHI.JKL" with the external IP address of your VM and then run this command:__

```
sudo docker run --rm -it \
    -p 17171:17171 \
    -p 17179:17179 \
    --add-host ABC.DEF.GHI.JKL.nip.io:0.0.0.0 \
    -e "XMAGE_DOCKER_SERVER_ADDRESS=ABC.DEF.GHI.JKL.nip.io" \
    goesta/xmage-beta
```

__7. Connect to server from an XMage client__
* Server name: `ABC.DEF.GHI.JKL`
* Port: `17171`

__8. Make sure to "delete" (not just "stop") the VM after use to avoid paying for storage while the server is offline.__


## Troubleshooting

__Ensure that the VM has an external IP address:__

Advanced Options > Networking > Network interfaces > "default" (or name of your network) > External IPv4 address > "Ephemeral"

__Unable to connect from XMage client:__

Review the firewall settings to allow incoming traffic.
