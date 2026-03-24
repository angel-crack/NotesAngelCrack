
```c
sudo apt update
sudo apt install flatpak -y
```


For Podman Desktop

```c
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

```c
flatpak remote-list
flatpak update
```

```c
flatpak install flathub org.freedesktop.Platform/x86_64/24.08
```

```c
flatpak install --user ~/Downloads/podman-desktop-1.17.2.flatpak
flatpak run io.podman_desktop.PodmanDesktop
```


