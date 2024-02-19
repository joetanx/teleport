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

To update user logins for Linux nodes

```console
[root@teleport ~]# tctl users update joe.tan --set-logins root,rhel-user
User joe.tan has been updated:
        New logins: root,rhel-user
```
