## 1. Install Teleport

### 1.1. Install Teleport binaries (`teleport`, `tsh`, `tctl`, `tbot`)

Ref: https://goteleport.com/docs/installation/#installation-instructions

```sh
curl https://goteleport.com/static/install.sh | bash -s 15.0.2
```

### 1.2. Configure teleport for SSL

Download [lab certificates](https://github.com/joetanx/lab-certs/)

```sh
curl -sLo /var/lib/teleport/fullchain.pem https://github.com/joetanx/lab-certs/raw/main/others/teleport.vx.pem
curl -sLo /var/lib/teleport/privkey.pem https://github.com/joetanx/lab-certs/raw/main/others/teleport.vx.key
```

Generate teleport configuration file

> [!Note]
>
> The `teleport configure -o file` command generates the config file at `/etc/teleport.vx` by default
>
> Omitting `-o file` generates the config file to `stdout`
>
> Use `-o file:///path/to/file` to generate the config file to `/path/to/file`

```sh
teleport configure -o file \
  --cluster-name=teleport.vx \
  --public-addr=teleport.vx:443 \
  --cert-file=/var/lib/teleport/fullchain.pem \
  --key-file=/var/lib/teleport/privkey.pem
```

### 1.3. Configure firewall

```sh
firewall-cmd --permanent --add-service https
firewall-cmd --permanent --add-port 3025/tcp
firewall-cmd --reload
```

### 1.4. Enable and start teleport service

```sh
systemctl enable --now teleport
```

### 1.5. Create user and first login

```console
[root@teleport ~]# tctl users add joe.tan --roles=editor,access --logins=root
User "joe.tan" has been created but requires a password. Share this URL with the user to complete user setup, link is valid for 1h:
https://teleport.vx:443/web/invite/5c1ba64daa8b0bd0abddb8305f8d5652

NOTE: Make sure teleport.vx:443 points at a Teleport proxy which users can access.
```

![image](https://github.com/joetanx/teleport/assets/90442032/befffe9b-d86e-4fdf-9d06-e0c4ec033347)

![image](https://github.com/joetanx/teleport/assets/90442032/27daeb1b-5748-4cd6-b889-b92b2f8ddc3a)

![image](https://github.com/joetanx/teleport/assets/90442032/45cd0047-319a-4a0c-a1f6-1c47a71c5c63)

![image](https://github.com/joetanx/teleport/assets/90442032/332b2d58-7b16-49c6-b439-2eb5180861b4)

## 2. Linux access

![image](https://github.com/joetanx/teleport/assets/90442032/55a3185b-4c0f-40f5-81cc-66027c3c5122)

![image](https://github.com/joetanx/teleport/assets/90442032/7da965d5-0b2e-45cc-84ae-46ee1cbe104a)

```console
[root@delta ~]# bash -c "$(curl -fsSL https://teleport.vx/scripts/e69058fc2e11c643bae3a151d404b471/install-node.sh)"
⋮
Installed:
  teleport-15.0.2-1.x86_64

Complete!
2024-02-19 22:05:36 +08 [teleport-installer] Found: Teleport v15.0.2 git:v15.0.2-0-g520f79d go1.21.7
2024-02-19 22:05:36 +08 [teleport-installer] Writing Teleport node service config to /etc/teleport.yaml

A Teleport configuration file has been created at "/etc/teleport.yaml".
To start Teleport with this configuration file, run:

teleport start --config="/etc/teleport.yaml"

Happy Teleporting!
2024-02-19 22:05:36 +08 [teleport-installer] Host is using systemd
2024-02-19 22:05:36 +08 [teleport-installer] Starting Teleport via systemd. It will automatically be started whenever the system reboots.
Created symlink /etc/systemd/system/multi-user.target.wants/teleport.service → /usr/lib/systemd/system/teleport.service.

Teleport has been started.

View its status with 'sudo systemctl status teleport.service'
View Teleport logs using 'sudo journalctl -u teleport.service'
To stop Teleport, run 'sudo systemctl stop teleport.service'
To start Teleport again if you stop it, run 'sudo systemctl start teleport.service'

You can see this node connected in the Teleport web UI or 'tsh ls' with the name 'delta.vx'
Find more details on how to use Teleport here: https://goteleport.com/docs/user-manual/

[root@delta ~]# useradd rhel-user
```

![image](https://github.com/joetanx/teleport/assets/90442032/e4faf926-b7dc-4f28-bf43-5efccfed92ed)

![image](https://github.com/joetanx/teleport/assets/90442032/0ecc3f39-82c0-4de0-8170-26f36bb0d574)

![image](https://github.com/joetanx/teleport/assets/90442032/17490800-52d3-474b-99c2-c804b388c95f)

![image](https://github.com/joetanx/teleport/assets/90442032/2167fbe2-27a1-40af-adcf-35f2fcdfb159)

![image](https://github.com/joetanx/teleport/assets/90442032/d2377727-6427-4c71-a6c3-b89b30e7e95c)
