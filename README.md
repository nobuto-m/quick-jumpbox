Prepare boot SD card.

    $ xzcat ubuntu-22.04.1-preinstalled-server-armhf+raspi.img.xz \
        | sudo dd bs=4M of=/dev/mmcblk0 oflag=sync status=progress

Boot, SSH and update hostname.

    $ sudo hostnamectl set-hostname tokyo-jumpbox

Import SSH keys and disable password.

    $ ssh-import-id nobuto
    $ sudo passwd -d "$USER"

Apply security updates.

    $ sudo apt update
    $ sudo unattended-upgrade -d

Install packages.

    $ sudo apt install prometheus-node-exporter

Install and setup tailscale.

 - https://tailscale.com/download/

Generate SSH key.

    $ ssh-keygen -t ed25519

Copy the key to the remote.

    [remote] $ cat ~/.ssh/authorized_keys
    command="",restrict,port-forwarding ssh-rsa ...

Clone the repository.

    $ git clone https://github.com/nobuto-m/quick-jumpbox.git
    $ cd quick-jumpbox/

Prepare SSH client configuration.

    $ cp -v -i ssh_config ~/.ssh/config
    $ editor ~/.ssh/config

Verify the SSH configuration.

    $ ssh -T remote-box true

Enable Phone home service.

    $ mkdir -p ~/.config/systemd/user/
    $ cp -v -i phone-home.service ~/.config/systemd/user/

    $ systemctl --user enable --now phone-home

Enable user instance on boot.

    $ loginctl enable-linger "$USER"

Install piglow module.

    $ sudo apt install python3-pip python3-smbus python3-rpi.gpio
    $ sudo -H pip3 install piglow==1.2.5

Add the user to i2c group.

    $ sudo adduser "$USER" i2c

Enable Phone home indicator service and timer.

    $ mkdir -p ~/.local/bin/
    $ cp -v -i phone-home-indicator ~/.local/bin/
    $ chmod +x ~/.local/bin/phone-home-indicator

    $ cp -v -i phone-home-indicator.service ~/.config/systemd/user/
    $ cp -v -i phone-home-indicator.timer ~/.config/systemd/user/

    $ systemctl --user enable phone-home-indicator.service
    $ systemctl --user enable phone-home-indicator.timer

Reboot.

    $ sudo reboot
