# Laboratorio-9-Adri-n-Alc-ntara-
Comandos utilizados en las practicas de Linux

===================================
PRACTICA 1 - INSTALACION DE WEBMIN
===================================

# Actualizar el sistema
sudo apt update && sudo apt upgrade -y

# Instalar dependencias
sudo apt install wget apt-transport-https software-properties-common -y

# Descargar webmin
sudo wget http://www.webmin.com/download/deb/webmin-current.deb

# Descarga el paquete
sudo dpkg -i webmin-current.deb

# Corregir errores de dependencias
sudo apt -f install

# Instalar Webmin
sudo apt install webmin -y

# Verificar si Webmin funciona
sudo systemctl status webmin

# Ver versión instalada
dpkg -l | grep webmin

# ------ CONFIGURACIÓN DEL FIREWALL ------

# Abrir puerto 10000
sudo ufw allow 10000/tcp

# Recargar firewall
sudo ufw reload

# Activar firewall
sudo ufw enable

# Ver estado
sudo ufw status

# Acceso a Webmin
En el navegador: https://IP_DEL_SERVIDOR:10000

=================================================================
PRACTICA 2 - DESPLIEGUE DE UNA VM CON TERRAFORM EN DIGITAL OCEAN
=================================================================

# Preparar el sistema
sudo apt update && sudo apt upgrade -y 

# Agregar repositorio de Terraform
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

# Agrega terraform como fuente de instalacion
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

# Instalar Terraform
sudo apt update && sudo apt install terraform -y

# Verificar instalación
terraform -v

# Crear carpeta
mkdir terraform-do
cd terraform-do

# Crear archivo principal
nano main.tf

# Crear archivo donde guardar el token 
Dentro de la misma carpeta creada anteriormente
sudo nano terraform.tfvars 
do_token = “TU_TOKEN”

# Crear el token en Digital Ocean 
Digital Ocean, API, Generate new token, Ponerle nombre: token terra, Tiempo de expiración: no expire, Scopes: full access, Generate token 

# Crear llave ssh  
Digital ocean, Settings, Security, Add SSH key 
ssh-keygen, cd .ssh, ls, cat id_ed25519.pub,
La copiamos en digital ocean, ponemos un nombre, Add SSK key

# Inicializar Terraform
sudo terraform init

# Ver plan
sudo terraform plan

# Crear VM
sudo terraform apply 

# VALIDAR LA VM
ssh root@IP_DE_LA_VM o En DigitalOcean se vera el droplet creado 

====================================
PRACTICA 3 - INSTALACION DE ANSIBLE
====================================

# Actualizamos los repositorios e instalamos el paquete de Ansible 
sudo apt update && sudo apt upgrade -y
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install -y python3
sudo apt install ansible -y

# -------- CONFIGURAR CLIENTE LINUX --------- #

# Crear usuario ansible
adduser ansible1

# Dar permisos administrador
sudo usermod -aG sudo ansible1

# Sincronizar la llave ssh con el usurio
rsync --archive --chown-ansible1:ansible1 ~/.ssh /home/ansible1

# -------- CONFIGURAR CLIENTE WINDOWS --------- #

# Crear usuario
En PowerShell (Administrador):
net user ansible2 Password123 /add

# Dar permisos administrador
net localgroup Administrators ansible2 /add

# Activar WinRM
winrm quickconfig

# Permitir conexiones remotas
Enable-PSRemoting -Force

# -------- CREAR INVENTARIO DE ANSIBLE --------- #

# Crear carpeta y archivo 
sudo mkdir /etc/ansible
sudo nano /etc/ansible/hosts
Aqui se configura todo para poder hacer ping hacia las dos maquinas clientes

# Contenido
[win]
win1 ansible_host=IP_WINDOWS

# -------- PROBAR CONEXION --------- #

# Probar Linux
ansible linux -m ping

# Probar Windows
ansible win -m win_ping

=============================
PRACTICA 4 - COMANDOS AD-HOC
=============================

# Probar conexión
ansible linux -m ping
ansible win -m win_ping

# ------- MODULO WIN_COPY ------- #

# Crear el archivo en windows
Explorador de archivo, Local disco, Users, ansible2, luego desktop, clic derecho, new, text document

# Copiar archivo  
ansible win -i /etc/ansible/hosts -m win_copy -a “src=C:/Users/ansible2/Desktop/ansiblewindows.txt dest=C:/Users/ansible2/Documents/ remote_src=yes”

# ------- MODULO REBOOT ------- #

# ssh a la maquina de ansible
ssh ansible1@143.198.162.204 o desde nuestra maquina de digital ocean entramos al usuario su - ansible1

# Maquina local
ansible linux -i /etc/ansible/hosts -m reboot --become

=======================
PRACTICA 5 - PLAYBOOKS
=======================

 # -------- PLAYBOOK NOTEPAD ------- #

# Crear archivo Notepad
sudo nano instalar_notepad.yml

# Para ejecutarlo 
ansible-playbook instalar_notepad.yml

# -------- PLAYBOOK NGINX ------- #

# Crear archivo nginx
sudo nano instalr_nginx.yml

# Para ejecutarlo 
ansible-playbook instalar_nginx.yml

# Probar
En el navegador: IP_DEL_SERVIDOR_LINUX
