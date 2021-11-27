# Enable root login - Ubuntu 20.04
Video explicativo en el canal de youtube [click here](https://www.youtube.com/watch?v=qziz2iEyLcc) .
## custom.conf
```sh
cd /etc/gdm3
gedit custom.conf
En el archivo incluir la linea "AllowRoot=true"
```
## gdm-password
```sh
cd /etc/pam.d/
gedit gdm-password
```
