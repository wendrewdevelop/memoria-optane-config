# memoria-optane-config
Guide to configure optane memory on Ubuntu.


1. Instale o sistema e dê boot no sistema instalado (instale com a opção LVM ativa no disco)
2. Execute o comando: sudo fdisk -l e verifique qual é a partição da memoria optane (/dev/nvme0n1)
3. Abra o gerenciador de discos e formate o disco da memoria optane.
4. Execute o comando: sudo vgextend vgubuntu /dev/nvme0n1
5. Execute o comando: sudo lvcreate --type cache --cachemode writeback -l100%FREE -n root_cachepool /dev/vgubuntu/root /dev/nvme0n1
6. Execute o comando: sudo apt update
7. Em seguida, execute o comando: sudo apt install thin-provisioning-tools
8. Após isso, baixe o item nesse link: https://opvizor-perfanalyzer.s3-eu-west-1.amazonaws.com/thin-provisioning-tools
9. Na sequencia execute essa lista de comandos, na ordem:
	sudo chown root:root ~/Downloads/thin-provisioning-tools
	sudo chmod 0755 ~/Downloads/thin-provisioning-tools
	sudo mv ~/Downloads/thin-provisioning-tools /usr/share/initramfs-tools/hooks/
10. Após isso, execute o comando: sudo update-initramfs -k all -u
11. Para finalizar, veja se deu certo com o comando a seguir: sudo lvdisplay /dev/ubuntu-vg/root

O resultado deve ser algo do tipo:
<br>
### --- Logical volume --- <br>
  LV Path                /dev/vgubuntu/root <br>
  LV Name                root <br>
  VG Name                vgubuntu <br>
  LV UUID                NgELaU-hPqx-UcpE-mj80-LOPX-Pn5n-b96cJY <br>
  LV Write Access        read/write <br>
  LV Creation host, time ubuntu, 2022-06-13 22:23:34 -0300 <br>
  LV Cache pool name     root_cachepool_cpool <br>
  LV Cache origin name   root_corig <br>
  LV Status              available <br>
  - open                 1 <br>
  LV Size                930,05 GiB <br>
  Cache used blocks      8,57% <br>
  Cache metadata blocks  10,91% <br>
  Cache dirty blocks     0,44% <br>
  Cache read hits/misses 2008 / 12007 <br>
  Cache wrt hits/misses  8239 / 12631 <br>
  Cache demotions        0 <br>
  Cache promotions       18785 <br>
  Current LE             238094 <br>
  Segments               1 <br>
  Allocation             inherit <br>
  Read ahead sectors     auto <br> 
  - currently set to     256 <br>
  Block device           253:0 <br>
<br>
Os blocos de Cache devem estar listados no retorno do comando.
