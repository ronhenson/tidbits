# Move wsl to D: drive

```powershell
wsl --list
wsl  --shutdown
LXRunOffline move -n Debian -d D:\wsl\Debian\Debian
wsl -d Debian
```