## Step 1: Update Ubuntu Server

```
$ sudo apt update
$ sudo apt -y upgrade && sudo systemctl reboot
```

Add host

```
$ sudo -i
# echo '<IP ADDRESS ENDPOINT> <Host NAME ENDPOINT>' >> /etc/hosts
# echo '<IP ADDRESS NODE1> <Host NAME NODE1>' >> /etc/hosts
# echo '<IP ADDRESS NODE2> <Host NAME NODE2>' >> /etc/hosts
...
```

Configure Aunthentication

```
### All nodes ###
# ssh-keygen -t rsa
# cat /root/.ssh/id_rsa.pub

### In first node copy SSH Key from all nodes paste in authorized_keys file ###
# vi /root/.ssh/authorized_keys

### Other node copy authorized_keys from first node ###
# scp root@<IP FIRST NODE>:/root/.ssh/authorized_keys /root/.ssh/authorized_keys
```
