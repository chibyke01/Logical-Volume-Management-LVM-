# Logical-Volume-Management-LVM-
## Code to execute the steps


lsblk

sudo gdisk /dev/xvdf
sudo gdisk /dev/xvdg
sudo gdisk /dev/xvdh

lsblk

sudo yum install lvm2
sudo lvmdiskscan

sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1

sudo pvs

sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1

sudo vgs

sudo lvcreate -n db-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg

sudo lvs

sudo vgdisplay -v
sudo lsblk

sudo mkfs -t ext4 /dev/webdata-vg/db-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv

sudo mkdir -p /db

sudo mkdir -p /home/recovery/logs

sudo mount /dev/webdata-vg/db-lv /db

sudo rsync -av /var/log/. /home/recovery/logs/

sudo mount /dev/webdata-vg/logs-lv /var/log

sudo rsync -av /home/recovery/logs/. /var/log

sudo blkid

sudo vi /etc/fstab

sudo mount -a
sudo systemctl daemon-reload