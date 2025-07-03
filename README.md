# Workstation Setup

1. Crea tu clave SSH con: `ssh-keygen`
1. Agrega la clave SSH pública[^ssh_pub] del provisionador a:
    - [Bitbucket](https://bitbucket.org/account/settings/ssh-keys/)
    - [GitHub](https://github.com/settings/keys/)
1. Agrega tu clave SSH al agente para hacer _forwarding_
    - En Linux ejecuta: `ssh-add ~/.ssh/id_ed25519`
    - En WSL agrega a `~/.bashrc`:
    ```shell
    eval `ssh-agent -s`
    ssh-add ~/.ssh/id_ed25519
    ```
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
    localhost ansible_connection=local
    ```
1. Crea directorio para clonar repositorios:
    ```shell
    mkdir --parents ~/repositorios/
    ```
1. Clona el repo que contiene el _playbook_ de configuración y correlo:
    ```shell
    cd ~/repositorios/
    git clone git@github.com:IslasGECI/development_server_setup.git
    cd development_server_setup/
    ansible-playbook ansible/development.yml
    ```
1. Agrega las dependencias del cliente liviano
    ```shell
    cd ~/repositorios/
    git clone git@github.com:devarops/thin_client.git
    cd thin_client
    ansible-playbook ansible/development.yml
    ```
1. Verifica que tu cliente liviano cuenta con el softare requerido
    ```shell
    cd ~/repositorios/thin_client
    make check
    ```

    - Si la veridicación anterior falla, corrige `thin_client/ansible/development.yml`

1. Instala [dotfiles](https://github.com/devarops/dotfiles):
    ```shell
    cd ~/repositorios/
    git clone git@github.com:devarops/dotfiles.git
    cd dotfiles
    make install
    ```
1. Agrega tu [bóveda secreta](https://docs.google.com/document/d/1lY7ycXs4J8wp1OyJCmPsvfB7YdQqscqL52cIZxBP6Rw/).
1. Carga credenciales y ruta de `container_runner` en `.bashrc`, actualiza y reinicia el sistema
    ```
    echo "source $HOME/.vault/.secrets" >> $HOME/.bashrc
    echo 'export PATH="/root/.local/bin:$PATH"' >> $HOME/.bashrc
    sudo apt update
    sudo apt full-upgrade --yes
    sudo reboot now
    ```
1. Verifica que puedes reproducir los análisis del equipo:
    ```
    geci-testmake hola mundo
    ```
