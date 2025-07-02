# `devmachine`

Estoy tratando de instalar `docker compose` (con espacio, sin guión) en el _devserver_. Probaré el playbook de Ansible localmente para no tener que esperar al siguiente día que se cree el _devserver_.

## En Linux

Crea VM e instala Ansible:

```shell
multipass launch --name devmachine
multipass shell devmachine
sudo apt update && sudo apt install --yes ansible
```

Crea el archivo `/etc/ansible/hosts`:

```shell
sudo mkdir -p /etc/ansible/
sudo vi /etc/ansible/hosts
```

Con el siguiente contenido:

```ini
[devserver]
localhost ansible_connection=local
```

Clona el repo que contiene el _playbook_ de configuración y correlo:

```shell
git clone https://github.com/IslasGECI/development_server_setup.git
cd development_server_setup/
ansible-playbook ansible/development.yml
```

---

# En Windows

## En la `devmachine`:
```shell
echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIInL8Mh6IAcwozVqlYRoBtBkgjUG8zKeWzNVigS/GDoN ubuntu@provisioner" >> .ssh/authorized_keys 
```

## En el `provisioner`:
```shell
sudo apt update && sudo apt install --yes ansible
```

Crea el archivo `/etc/ansible/hosts`:

```shell
sudo mkdir -p /etc/ansible/
sudo vi /etc/ansible/hosts
```

Con el siguiente contenido:

```ini
[devserver]
172.21.45.121
```

Clona el repo que contiene el _playbook_ de configuración y correlo:

```shell
git clone https://github.com/IslasGECI/development_server_setup.git
cd development_server_setup/
ansible-playbook ansible/development.yml
```
