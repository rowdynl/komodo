# Stack Media
This is the folder for the `media` stack. This includes `Plex`, the `*arr` services and some download clients.

## Plex
Ubuntu 25.10 uses the newer XE driver that is not (fully) supported by Plex. This is a known issue. To fix this, I've edited the grub config to force the 'old' i915 driver.

```
sudo vi /etc/default/grub
```

Add to GRUB_CMDLINE_LINUX_DEFAULT:

```
i915.force_probe=* module_blacklist=xe
```

After that:

 ``` 
sudo update-grub
sudo reboot
```