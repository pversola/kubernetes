## Step 3: Disable Swap

```
$ sudo sed -ri '/\sswap\s/s/^#?/#/' /etc/fstab
$ sudo swapoff -a
```
