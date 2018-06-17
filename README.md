Prepare boot SD card.

    $ xzcat ubuntu-18.04-preinstalled-server-armhf+raspi2.img.xz \
        | sudo dd bs=4M of=/dev/mmcblk0 oflag=sync status=progress

Boot, SSH and update hostname.

    $ sudo hostnamectl set-hostname tokyo-jumpbox

Import SSH keys and disable password.

    $ ssh-import-id nobuto
    $ sudo passwd -d "$USER"

Apply security updates.

    $ sudo apt update
    $ sudo unattended-upgrade -d

Generate SSH key.

    $ ssh-keygen

Copy the key to the remote.

    [remote] $ cat ~/.ssh/authorized_keys
    command="",restrict,port-forwarding ssh-rsa ...

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
