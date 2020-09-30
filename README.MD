# Instalação do Arch Linux


Instalação simples do Arch Linux que utilizo para pc e notebook.

Esse é um resumo, se tiver dúvidas, leia a documentação!




# Passos iniciais 

**teclado em pt-br:** 

    loadkeys br-abnt2

**Configuração do mirror para download mais rápido:** 

    nano /etc/pacman.d/mirrorlist

**Se for utilizar wifi:**

    wifi-menu

**Formatação do disco:**

    cfdisk -z  / ou se desejar selecionar o disco cfdisk -z /dev/sda


**Partições básicas:**

> **Nota:** Não é necessário criar a partição home, se desejar ou tiver pouco espaço, crie apenas a partição **RAIZ**, mas se desejar futuramente formatar apenas o sistema sem perder as fotos, vídeos e arquivos, crie a **HOME**

| Nome das partições| Tamanho |
|--|--|
| **BIOS BOOT** | **500M**  /dev/sda1| 
| **RAIZ** | **100/200G** /dev/sda2|
| **HOME** | **TODO DISCO ** /dev/sda3|
| **SWAP** | **4 A 8 GB** /dev/sda4|

**Formatando as partições:**

>**Nota:** Preste atenção na numeração, pois se não criar a partição home, irá mudar `/dev/sdaX`



**BIO BOOT:** `mkfs.fat -F32 /dev/sda1`
**RAIZ:** `mkfs.ext4  /dev/sda2`
**HOME:** `mkfs.ext4  /dev/sda3`
**SWAP:** `mkswap /dev/sda4`

**Montando as partições:**

**RAIZ:** `mount  /dev/sda2 /mnt`

**HOME:** Crie  a pasta `mkdir /mnt/home`  e monte a mesma `mount /dev/sda3 /mnt/home`

**SWAP:** `swapon /dev/sda4`

## Instalação de packages essenciais 

    pacstrap /mnt linux linux-firmware base base-devel dhcpcd nano man-db

## Gerando o arquivo `fstab`

    genfstab -U /mnt >> /mnt/etc/fstab
    
Para checar se foi gerado o arquivo

    cat /mnt/etc/fstab

## Entrando no sistema e configurando

**Para entrar digite:**  `arch-chroot /mnt`

**Horário do sistema:**`ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime`

**Para iniciar:** `hwclock --systohc`

**Idioma do sistema:**
1- `nano /etc/locale.gen` 
2- Desmarque o pt_BR.UTF-8 e execute `locale-gen`

**Crie um arquivo:** `nano /etc/locale.conf` e adicione `LANG=pt_BR.UTF-8`

**Layout do teclado:** `nano /etc/vconsole.conf` e adicione `KEYMAP=br-abnt2`


**Setando hostname:** `nano /etc/hostname` e coloque um nome 
ex: nomedousuario@**hostname**

**hosts:** `nano /etc/hosts` e adicione as três linhas a baixo

    127.0.0.1	localhost.localdomain	loclhost
    
    ::1	localhost.localdomain	loclhost
    
    127.0.1.1	seuhostname.localdomain	seuhostname


## Pacotes básicos para funcionar a internet/wireless e últimas configurações

**Internet a cabo:** `pacman -S networkmanager`

**Sem fio:** `pacman -S network-manager-applet networkmanager wpa_supplicant wireless_tools`

**Pacotes básicos** `pacman -S dialog vim xorg xorg-server curl wget git sudo mesa libgl xfce4-notifyd gnome-keyring`

> **Nota:** Adicione o que desejar ou retire se não for utilizar algo, estou resumindo minha instalação aqui.
    
**Forçando a inicialização da internet e gerenciador de interface**

    systemctl enable NetworkManager.service
    systemctl enable lightdm.service

**Senha do root:** `passwd`
**Criando um usuário:** useradd -m -G  wheel -s /bin/bash nomedouser
**Criando uma senha:** `passwd nomedouser`
**Adicionando o usuário ao sudoers** `nano /etc/sudoers` e procure por `root ALL=(ALL) ALL` e adicione uma linha a baixo apenas trocando para o nome do seu usuário
**Gerando novamente um script RAM inicial:** `mkinitcpio -p linux`
>**Nota:** Esse  último passo não é necessário, eu apenas aprendi desde o começo  a utilizar e gosto de manter minhas installs assim.

**Instalando e configurando grub:** `grub-install /dev/sda`  e `grub-mkconfig -o /boot/grub/grub.cfg`

>**ATENÇÃO:** Após completar todos esses passos, você pode escolher uma interface como XFCE, KDE PLASMA, GNOME entre outas ou optar pelo I3WM, OPENBOX, basta instalar e depois instalar ou não os drives da sua placa de vídeo  e reiniciar que o sistema estará pronto


>**Exemplo instalação drive intel:** `pacman -S xf86-video-intel`
