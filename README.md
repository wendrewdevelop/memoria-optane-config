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

--- Logical volume ---
  LV Path                /dev/vgubuntu/root
  LV Name                root
  VG Name                vgubuntu
  LV UUID                NgELaU-hPqx-UcpE-mj80-LOPX-Pn5n-b96cJY
  LV Write Access        read/write
  LV Creation host, time ubuntu, 2022-06-13 22:23:34 -0300
  LV Cache pool name     root_cachepool_cpool
  LV Cache origin name   root_corig
  LV Status              available
  # open                 1
  LV Size                930,05 GiB
  Cache used blocks      8,57%
  Cache metadata blocks  10,91%
  Cache dirty blocks     0,44%
  Cache read hits/misses 2008 / 12007
  Cache wrt hits/misses  8239 / 12631
  Cache demotions        0
  Cache promotions       18785
  Current LE             238094
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

Os blocos de Cache devem estar listados no retorno do comando.
