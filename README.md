# Instalação do Arch Linux

### "Observação: Todos os pacotes e configurações aqui postados são necessários para executar o sistema sem problemas." ###

## 1° Passo: Deixar o teclado em pt_BR: ##
```loadkeys br-abnt2```

## 2° Passo: Testar e ativar conexão com a internet: ##
```ping -c3 google.com.br```
```systemctl status dhcpcd```
```systemctl start dhcpcd```
```systemctl status dhcpcd```

## 3° Passo: Particionar o disco: ##
*Pessoalmente, eu crio sempre duas partições:*
```/dev/sda1 – 490GB```
```/dev/sda2 – 10GB```

*A partição ```/dev/sda1``` deve ser primária e bootável. 
A partição ```/dev/sda2``` deve ser primária e do tipo ```Linux Swap / Solaris```:*
*Depois de ter feito isso, basta dar ```write``` e ```quit```*.

## 4° Passo: Formatar as partições criadas: ##
```mkfs.ext4 /dev/sda1```
```mount -t ext4 /dev/sda1 /mnt```
```mkswap /dev/sda2```
```swapon /dev/sda2```

## 5° Passo: Definir um servidor de download: ##
```nano /etc/pacman.d/mirrorlist```

*Procure por Brazil, recorte o espelho e cole no inicio do arquivo.
O espelho se parece com isso:*
```"Server = http://mirror.ufscar.br/archlinux/$repo/os/$arch".```

## 6° Passo: Instalar o sistema base: ##
```pacstrap /mnt base base-devel```

## 7° Passo: Gerar o fstab: ##
```genfstab /mnt >> /mnt/etc/fstab```

## 8° Passo: Logar no chroot: ##
```arch-chroot /mnt```

## 9° Passo: Definir o idioma: ##
```nano /etc/locale.gen```

*Procure por ```pt_BR```, você vai achar a seguinte linha  ```#pt_BR.UTF-8 UTF-8```, basta remover o sustenido ```#``` da frente dela e inserir espaço:*

```locale-gen```
```echo LANG=pt_BR.UTF-8 > /etc/locale.conf```
```export LANG=pt_BR.UTF-8```

## 10° Passo: Definir horário: ##
```rm /etc/localtime```
```ln -s /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime```

## 11° Passo: Definir senha do root: ##
```passwd```

## 12° Passo: Criar um usuário e definir sua senha: ##
```useradd -m -g users -G wheel -s /bin/bash SEU-USUÁRIO```
```passwd SEU-USUÁRIO```

## 13° Passo: Baixar e instalar o grub: ##
```pacman -S grub```
```grub-install /dev/sda```

## 14° Passo: Rodar os dois últimos comandos: ##
```mkinitcpio -p linux```
```grub-mkconfig -o /boot/grub/grub.cfg```

## 15° Passo: Saia do chroot: ##
```exit```

## 16° Passo: Desmonte todas as partições e reinicie o sistema: ##
```umount -a```
```reboot```

## 17° Passo: Continuação sugerida: ##
[INSTALAÇÃO DO ARCH LINUX COM O I3-GAPS](https://github.com/jirrezdex/archlinux)