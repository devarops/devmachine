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

## En la `devmachine`

1. 
    ```shell
    echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIInL8Mh6IAcwozVqlYRoBtBkgjUG8zKeWzNVigS/GDoN ubuntu@provisioner" >> .ssh/authorized_keys 
    ```

## En el `provisioner`

1. Instala Ansible
    ```shell
    sudo apt update && sudo apt install --yes ansible
    ```
1. Crea el archivo `/etc/ansible/hosts`:
    ```shell
    sudo mkdir -p /etc/ansible/
    sudo vi /etc/ansible/hosts
    ```
1. Con el siguiente contenido:
    ```ini
    [devserver]
    172.21.45.121
    ```
1. Crea directorio para clonar repositorios:
    ```shell
    mkdir --parents ~/repositorios/
    ```
1. Clona el repo que contiene el _playbook_ de configuración y correlo:
    ```shell
    cd ~/repositorios/
    git clone https://github.com/IslasGECI/development_server_setup.git
    cd development_server_setup/
    ansible-playbook ansible/development.yml
    ```

## En la `devmachine`

1. Verifica que tu cliente liviano cuenta con el softare requerido
    ```shell
    cd ~/repositorios/
    git clone git@github.com:devarops/thin_client.git
    cd thin_client
    make check
    ```
    - Instalación del `rich`:

      ```
      sudo apt install pipx --yes
      pipx install rich-cli
      pipx ensurepath
      ```
      
   - En el `~/.bashrc` mover al inicio del archivo las líneas incertadas por `pipx`.
   
1. Instala [dotfiles](https://github.com/devarops/dotfiles):
    ```shell
    cd ~/repositorios/
    git clone git@github.com:devarops/dotfiles.git
    cd dotfiles
    make install
    ```
1. Agrega tu [bóveda secreta](https://docs.google.com/document/d/1lY7ycXs4J8wp1OyJCmPsvfB7YdQqscqL52cIZxBP6Rw/).
1. Copia las credenciales hacia `devmachine`
    ```shell
    ssh-keygen -f "$HOME/.ssh/known_hosts" -R "islasgeci.dev"
    ssh-keyscan "islasgeci.dev" >> "$HOME/.ssh/known_hosts"
    export DEVELOPER=<Tu nombre de usuario del servidor>
    scp -pr ~/.vault $DEVELOPER@islasgeci.dev:/home/$DEVELOPER/.vault
    scp ~/todo.md $DEVELOPER@islasgeci.dev:/home/$DEVELOPER/todo.md
    ```
1. Desde `devmachine`, configura el servidor de desarrollo
    ```shell
    cd ~/repositorios/thin_client
    make setup_server
    ```
