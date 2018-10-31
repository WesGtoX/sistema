# <h1 align='center'>Instalação do Arch Linux</h1>
<br>
<br>
<p><b>"Observação: Todos os pacotes e configurações aqui postados são necessários para executar o sistema sem problemas."</b>
<br>
<br>
<b>1° Passo: Deixar o teclado em pt_BR.</b><br>
loadkeys br-abnt2<br><br>
<b>2° Passo: Testar conexão com a internet e ativar.</b><br>
ping -c3 google.com.br<br>
systemctl status dhcpcd<br>
systemctl start dhcpcd<br>
systemctl status dhcpcd<br><br>
<b>3° Passo: Parcionar o disco.</b><br>
Pessoalmente, eu crio sempre duas partições - /dev/sda1 com 490GB - /dev/sda2 com 10GB.<br>
<b>A partição /dev/sda1 deve ser primária e bootável.<br>A partição /dev/sda2 deve ser primária e do tipo Linux Swap / Solaris.</b><br>
Depois de ter feito isso, basta dar "write" e "quit"<br><br>
<b>4° Passo: Formatar as partições criadas.</b><br>
mkfs.ext4 /dev/sda1<br>
mount -t ext4 /dev/sda1 /mnt<br>
mkswap /dev/sda2<br>
swapon /dev/sda2<br><br>
<b>5° Passo: Definir um servidor de download.</b><br>
nano /etc/pacman.d/mirrorlist<br>
<b>Procure por Brazil, recorte o comando e cole no inicio do arquivo.</b><br><br>
<b>6° Passo: Instalar o sistema base.</b><br>
pacstrap /mnt base base-devel<br><br>
<b>7° Passo: Gerar o fstab.</b><br>
genfstab /mnt >> /mnt/etc/fstab<br><br>
<b>8° Passo: Logar no chroot.</b><br>
arch-chroot /mnt<br><br>
<b>9° Passo: Definir o idioma.</b><br>
nano /etc/locale.gen<br>
<b>Procure por pt_BR, você vai achar a seguinte linha  #pt_BR.UTF-8 UTF-8, basta remover o sustenido da frente dela e inserir espaço.</b><br>
locale-gen<br>
echo LANG=pt_BR.UTF-8 > /etc/locale.conf<br>
export LANG=pt_BR.UTF-8<br><br>
<b>10° Passo: Definir horário.</b><br>
rm /etc/localtime<br>
ln -s /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime<br><br>
<b>11° Passo: Definir senha do root.</b><br>
passwd<br><br>
<b>12° Passo: Criar um usuário e definir sua senha.</b><br>
useradd -m -g users -G wheel -s /bin/bash SEU-USUÁRIO<br>
passwd SEU-USUÁRIO<br><br>
<b>13° Passo: Baixar o grub e instalar.</b><br>
pacman -S grub<br>
grub-install /dev/sda<br><br>
<b>14° Passo: Rodar dois ultimos comandos.</b><br>
mkinitcpio -p linux<br>
grub-mkconfig -o /boot/grub/grub.cfg<br><br>
<b>15° Passo: Saia do chroot.</b><br>
exit<br><br>
<b>16° Passo: Desmonte todas as partições e reinicie o sistema.</b><br>
umount -a<br>
reboot<p>