# rocketpool-vps-guide


### Initial provider comparison of VPS nodes that meet the Rocket Pool requirements.

| Provider     | Model                                                                                                                                                                | CPU | RAM (GB) | SSD Storage (GB) | Price (Month) USD |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- | -------- | ---------------- | ----------------- |
| netcup       | [VPS 4000 G9](https://www.netcup.eu/bestellen/produkt.php?produkt=2602)                                                                                              | 8   | 32       | 800              | $25.00            |
| Hetzner      | [CPX41](https://www.hetzner.com/cloud)                                                                                                                               | 8   | 16       | 120 +200         | $37.00            |
| OVH          | [Comfort](https://us.ovhcloud.com/order/vps/?v=3#/vps/build?selection=~(range~'Comfort~pricingMode~'default~flavor~'vps-comfort-4-8-160~datacenters~(US-EAST-VA~1))) | 4   | 8        | 160 +200         | $45.00            |
| So you Start | [E3-SSD-1-32](https://www.soyoustart.com/us/order/soYouStart.xml?reference=1804sys47)                                                                                | 4   | 32       | 480              | $48.00            |
| Linnode      | N/A                                                                                                                                                                  | 6   | 16       | 320              | $80.00            |
| AWS      | [t2.large](https://aws.amazon.com/ec2/instance-types/t2/)                                                                                                                | 2   | 8       | 400              | $132.00            |


## Hardening

For all VPS provides always change passwords for the management portals.


#### Contabo Specific Portal Settings

##### Disable VNC:
VPS control > Manage > Disable VNC

<br />

#### Accounts & SSH

Change Root Password: `passwd`

Create dedicated user with sudo:
```
adduser rocket
usermod -aG sudo rocket
```

Disable root ssh after verifying ssh works from newly created user:
```
sudo vi /etc/ssh/sshd_config
```
Change `PermitRootLogin yes` to `PermitRootLogin no`

Reload sshd:
```
sudo service sshd reload
```

##### Bonus Section:
Deploy SSH keys and disable SSH Password Authentication

Copy local ssh pubic ket to server with `ssh-copy-id <username>@<server>`
Verify passwordless login is now working as expected.

Edit ssh_config `sudo vi /etc/ssh/sshd_config`  
Change the following values to `No` to disable SSH login via password:
```
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no
```
Reload sshd:
```
sudo service sshd reload
```

<br />

### Firewall

Setup default UFW rules:
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Allow SSH:
```
sudo ufw allow ssh
```

Allow Rocket Pool Ports:
```
sudo ufw allow 30303
sudo ufw allow 9001
```

Enable UFW and accept prompt:
```
sudo ufw enable
```

Check UFW Rules:
`sudo ufw status`

```
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
30303                      ALLOW       Anywhere
9001                       ALLOW       Anywhere
22/tcp (v6)                ALLOW       Anywhere (v6)
30303 (v6)                 ALLOW       Anywhere (v6)
9001 (v6)                  ALLOW       Anywhere (v6)
```
