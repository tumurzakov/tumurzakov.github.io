**Quick memo: How to turn on file swap on linux server.** Today I worked with server where by some reason was turned off swap. It seems that this machine was targeted as k8s worker node but then was repurposed as database server. It is good idea to have swap turned on there.

```bash
# sudo fallocate -l 4G /swap.img
# sudo chmod 600 /swap.img 
# sudo mkswap /swap.img 
# sudo swapon /swap.img 
# sudo echo "/swap.img       none    swap    sw      0       0" >> /etc/fstab
```
