# DHCP con Vagrant

Práctica para crear una red con Vagrant y VirtualBox.

## Requisitos

- Vagrant
- VirtualBox
- Un adaptador de red configurado en el equipo

## Máquinas virtuales

El proyecto crea tres máquinas Debian:

- `server`: servidor DHCP con la IP `192.168.57.10`.
- `client`: cliente de la red interna.
- `printer`: impresora configurada para obtener una IP por DHCP.

La red interna utilizada se llama `intnet`.

## Iniciar las máquinas

Desde la carpeta del proyecto:

```bash
vagrant up
```

Para comprobar el estado:

```bash
vagrant status
```

## Configurar el servidor DHCP

Entrar en el servidor:

```bash
vagrant ssh server
```

Instalar el servicio DHCP:

```bash
sudo apt update
sudo apt install isc-dhcp-server
```

Hacer una copia del archivo de configuración:

```bash
sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bak
```

Editar la configuración:

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Añadir lo siguiente:

```text
default-lease-time 86400;
max-lease-time 691200;

subnet 192.168.57.0 netmask 255.255.255.0 {
    range 192.168.57.20 192.168.57.50;
    option routers 192.168.57.10;
    option subnet-mask 255.255.255.0;
}
```

Reiniciar el servicio DHCP:

```bash
sudo systemctl restart isc-dhcp-server
sudo systemctl status isc-dhcp-server
```

## Comprobar la red

Entrar en el cliente:

```bash
vagrant ssh client
```

Consultar las interfaces y la dirección asignada:

```bash
ip a
```

Comprobar la conexión con el servidor:

```bash
ping 192.168.57.10
```

## Detener y eliminar las máquinas

Para detener las máquinas:

```bash
vagrant halt
```

Para eliminarlas:

```bash
vagrant destroy
```

La carpeta `.vagrant` está incluida en `.gitignore` y no se sube al repositorio.
