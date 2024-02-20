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

### 2.1. Using Teleport Connect client

![image](https://github.com/joetanx/teleport/assets/90442032/9efc7a99-6b7f-4534-8259-92d2a349b814)

![image](https://github.com/joetanx/teleport/assets/90442032/f6eda8d3-e698-4d94-b6c7-f244e588310d)

![image](https://github.com/joetanx/teleport/assets/90442032/ca51f841-e30c-480c-815f-6a6fdbdf2ff1)

![image](https://github.com/joetanx/teleport/assets/90442032/61a62322-4595-4c07-96f8-1bf6bd27e201)

![image](https://github.com/joetanx/teleport/assets/90442032/f952507e-85e9-4c1a-874e-172084c0a148)

![image](https://github.com/joetanx/teleport/assets/90442032/65697c02-ef6a-4553-ac97-7f5799ddef7a)

![image](https://github.com/joetanx/teleport/assets/90442032/7ca5c165-a00b-4a5e-a907-dfdc48b98dc7)

### 2.2. Using tsh CLI client

![image](https://github.com/joetanx/teleport/assets/90442032/d2659e04-674e-402c-9b45-2002fdb1ec23)

![image](https://github.com/joetanx/teleport/assets/90442032/a1ed6c2f-b975-481c-a9a8-bbe1cc58331a)

## 3. Active Directory access

### 3.1. Add Active Directory resource

Ref: https://goteleport.com/docs/desktop-access/active-directory/

#### 3.1.1. Prepare Active Directory

![image](https://github.com/joetanx/teleport/assets/90442032/f6a9d407-43a0-4b7b-a2a5-364255cedd08)

![image](https://github.com/joetanx/teleport/assets/90442032/a8fd70c2-9a65-4330-bcf1-77fd3a980876)

#### 3.1.2. Configure the Windows Desktop Service

```console
[root@teleport ~]# cat << EOF >> /etc/teleport.yaml
windows_desktop_service:
  enabled: yes
  listen_addr: '0.0.0.0:3028'
  ldap:
    addr:     '192.168.17.201:636'
    domain:   'ark.vx'
    username: 'ARK\svc-teleport'
    sid: 'S-1-5-21-170323617-2561588915-2947752421-2101'
    server_name: 'HQ.ark.vx'
    insecure_skip_verify: false
    ldap_ca_cert: |
        -----BEGIN CERTIFICATE-----
        MIIB6zCCAXCgAwIBAgIUY7VqldG2k1klQDA1pzUeefdV4r4wCgYIKoZIzj0EAwIw
        FjEUMBIGA1UEAwwLTGFiIFJvb3QgQ0EwHhcNMTkxMjMxMTYwMDAwWhcNNDkxMjMx
        MTYwMDAwWjA/MRMwEQYKCZImiZPyLGQBGRYDdnggMRQwEgYKCZImiZPyLGQBGRYE
        YXJrIDESMBAGA1UEAwwJYXJrLUhRLUNBMHYwEAYHKoZIzj0CAQYFK4EEACIDYgAE
        5bR7jFkT4qD4QbS8WJVjIMe3kh9n7P/9YCR4msAtvTC38TQrVFjCw71AmBbreyh2
        vlCCxJQdcQt+y5OVGLb1mGBBTIB9e8vjYFxm2AGBgEVZowXzPTFLGwDzugG8ZusM
        o1YwVDASBgNVHRMBAf8ECDAGAQH/AgEAMB0GA1UdDgQWBBR31JY+KxatXokXK9bU
        EvjeWmJEQDAfBgNVHSMEGDAWgBTwrZ5zZKHFEJsm0AIo49dbC+7S8DAKBggqhkjO
        PQQDAgNpADBmAjEA1U+pkTyV7sgm8oJA03a4RvMzL/qx+SN3GjpYL1P9DKAKaPMA
        w19hRpfq/BlcM09jAjEArT6EhCw0ABotBC1fgTRxQ5PvlknwXO8zvQkIhcho0mcj
        GMIdS2ANHriNOaMDg+NM
        -----END CERTIFICATE-----

  discovery:
    base_dn: '*'
  labels:
    teleport.internal/resource-id: c165b459-29a7-43bf-ac24-8149ce622090
EOF
[root@teleport ~]# firewall-cmd --permanent --add-port 3028/tcp && firewall-cmd --reload
success
success
[root@teleport ~]# systemctl restart teleport
[root@teleport ~]# tctl users update joe.tan --set-windows-logins domainadmin
User joe.tan has been updated:
        New Windows logins: domainadmin
```

#### 3.1.3. Verify resource enrollment

![image](https://github.com/joetanx/teleport/assets/90442032/ce262113-c2bf-4f68-b670-91fc9a418cf9)

![image](https://github.com/joetanx/teleport/assets/90442032/5234b009-d929-49db-813d-5d7f7c748bd6)

![image](https://github.com/joetanx/teleport/assets/90442032/ea4d203b-43f7-408e-8cd1-30f2b6d29f1d)

### 3.2. RDP connection

![image](https://github.com/joetanx/teleport/assets/90442032/3cddf3a2-35d7-4483-93bb-6c3afaa23c45)

![image](https://github.com/joetanx/teleport/assets/90442032/bf0cd3e3-fc09-4e5d-ab96-470bb3eda727)

![image](https://github.com/joetanx/teleport/assets/90442032/10faf8b4-39ae-42ce-8a9e-4f1cc075cf16)

The access to `domainadmin` account is enabled by client certificate issued by Teleport

![image](https://github.com/joetanx/teleport/assets/90442032/f20b009f-96d5-4086-89e1-ab2dd8201af7)

![image](https://github.com/joetanx/teleport/assets/90442032/32fa048c-757b-439e-b495-2be99e79ccb5)

The Teleport CA is added to target resources during the installation procedure

![image](https://github.com/joetanx/teleport/assets/90442032/1933f0df-bcb0-402e-992e-d537c7f3f6a5)

File transfer can be done by sharing a directory from client machine to target resource

![image](https://github.com/joetanx/teleport/assets/90442032/cf3fc5e1-7259-4327-bc17-ef839bd811fe)

![image](https://github.com/joetanx/teleport/assets/90442032/504d9600-778a-4ea3-8d5c-d0f9404fda11)

## 4. Windows local access

### 4.1. Add Windows local resource

Ref: https://goteleport.com/docs/desktop-access/getting-started/

#### 4.1.1. Prepare Windows

Export the Teleport user certificate authority

```sh
curl -ko teleport.cer https://teleport.vx/webapi/auth/export?type=windows
```

Download and run the Teleport Windows Auth setup program
```sh
curl -o teleport-windows-auth-setup.exe https://cdn.teleport.dev/teleport-windows-auth-setup-v15.0.2-amd64.exe
teleport-windows-auth-setup.exe install --cert=teleport.cer -r
```

#### 4.1.2. Configure the Windows Desktop Service

> [!Note]
>
> The Windows Desktop Service is running on the Teleport proxy itself. Hence, no token generation is required.
>
> If the Windows Desktop Service is running separate from the Teleport proxy follow the instruction in the referred link to perform `tctl tokens add --type=windowsdesktop`

```console
[root@teleport ~]# cat << EOF >> /etc/teleport.yaml
windows_desktop_service:
  enabled: yes
  listen_addr: '0.0.0.0:3028'
  static_hosts:
  - name: cybr-ark-vx
    ad: false
    addr: 192.168.17.202
EOF
[root@teleport ~]# firewall-cmd --permanent --add-port 3028/tcp && firewall-cmd --reload
success
success
[root@teleport ~]# systemctl restart teleport
[root@teleport ~]# tctl create -f - << EOF
kind: role
version: v6
metadata:
  name: windows-desktop-admins
spec:
  allow:
    windows_desktop_labels:
      "*": "*"
    windows_desktop_logins: ["Administrator", "localadmin"]
EOF
role 'windows-desktop-admins' has been created
[root@teleport ~]# tctl users update joe.tan --set-roles=editor,access,windows-desktop-admins
User joe.tan has been updated:
        New roles: editor,access,windows-desktop-admins
```

### 4.2. RDP connection

![image](https://github.com/joetanx/teleport/assets/90442032/6af3ea53-71a3-46de-aa70-349e5f41c1f7)

![image](https://github.com/joetanx/teleport/assets/90442032/2a4a5e24-7621-40a4-a481-439d0a9d16b3)

![image](https://github.com/joetanx/teleport/assets/90442032/909a4f17-2938-42c7-8b54-5936f5fa6dd6)

The access to `localadmin` account is enabled by client certificate issued by Teleport

![image](https://github.com/joetanx/teleport/assets/90442032/6a7d8c7e-4b2e-4775-928f-440c058a9392)

![image](https://github.com/joetanx/teleport/assets/90442032/69d7d08b-8850-4925-aa42-2f8ca4a2d9d0)

The Teleport CA is added to target resources during the installation procedure

![image](https://github.com/joetanx/teleport/assets/90442032/f70eff7a-39e8-4796-ab8e-061357aaf8fb)

File transfer can be done by sharing a directory from client machine to target resource

![image](https://github.com/joetanx/teleport/assets/90442032/a89cb41b-1676-4ffe-92e3-1d73aa2db6bc)

![image](https://github.com/joetanx/teleport/assets/90442032/742e9b93-1778-492d-914b-bde468af084e)

## 5. Kubernetes access

### 5.1. Add Kubernetes resource

![image](https://github.com/joetanx/teleport/assets/90442032/f6cca001-e631-402b-b878-8659bc628cde)

![image](https://github.com/joetanx/teleport/assets/90442032/e7912755-cb4e-45d9-b5cd-aeadc299d5b0)

### 5.2. Configure Kubernetes cluster

```console
[root@kube ~]# helm repo add teleport https://charts.releases.teleport.dev && helm repo update
"teleport" has been added to your repositories
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "teleport" chart repository
Update Complete. ⎈Happy Helming!⎈
[root@kube ~]# cat << EOF > prod-cluster-values.yaml
roles: kube,app,discovery
authToken: 3761129c86b19bfe78b9b2067faa65e6
proxyAddr: teleport.vx:443
kubeClusterName: kube.vx
labels:
    teleport.internal/resource-id: 44821782-e00e-4f81-8f50-2fde435f3935
EOF
[root@kube ~]# helm install teleport-agent teleport/teleport-kube-agent -f prod-cluster-values.yaml --version 15.0.1 --create-namespace --namespace teleport
NAME: teleport-agent
LAST DEPLOYED: Fri Feb 16 09:30:21 2024
NAMESPACE: teleport
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:

[root@kube ~]# kubectl apply -f - <<EOF
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: viewers-crb
subjects:
- kind: Group
  # Bind the group "viewers", corresponding to the kubernetes_groups we assigned our "kube-access" role above
  name: viewers
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  # "view" is a default ClusterRole that grants read-only access to resources
  # See: https://kubernetes.io/docs/reference/access-authn-authz/rbac/#user-facing-roles
  name: view
  apiGroup: rbac.authorization.k8s.io
EOF
clusterrolebinding.rbac.authorization.k8s.io/viewers-crb created
```

> [!Note]
>
> TLS errors like the one below can occur when using self-signed or internal CA certificates
>
> ```
> 2024-02-16T01:30:37Z ERRO [PROC:1]    Discovery failed to establish connection to cluster: Post "https://teleport.vx:443/v1/webapi/host/credentials": tls: failed to verify certificate: x509: certificate signed by unknown authority. pid:2.1 service/connect.go:109
> 2024-02-16T01:30:37Z ERRO [PROC:1]    App failed to establish connection to cluster: Post "https://teleport.vx:443/v1/webapi/host/credentials": tls: failed to verify certificate: x509: certificate signed by unknown authority. pid:2.1 service/connect.go:109
> ```
>
> Create a ConfigMap for your self-signed/internal CA certificate and update the `teleport-agent` statefulset configuration
>
> ```console
> kubectl -n teleport create configmap ca-pemstore --from-file=/etc/pki/ca-trust/source/anchors/lab_root.pem
> kubectl -n teleport patch statefulset teleport-agent --patch '{"spec":{"template":{"spec":{"containers":[{"name": "teleport","volumeMounts": [{"mountPath": "/etc/ssl/certs/lab_root.pem","name": "ca-pemstore","subPath": "lab_root.pem","readOnly": true}]}],"volumes": [{"configMap": {"name": "ca-pemstore"},"name": "ca-pemstore"}]}}}}'
> kubectl -n teleport delete pod teleport-agent-0
> ```

The wizard should detect the Kubernetes cluster if the `teleport-agent` statefulset connection is successful

![image](https://github.com/joetanx/teleport/assets/90442032/c1e5fccb-eaa7-4ef2-9019-cdaa2be09bcc)

### 5.3. Setup access to the Kubernetes cluster

![image](https://github.com/joetanx/teleport/assets/90442032/301cd519-ab40-46f1-a7a2-bfa4edfc1302)

![image](https://github.com/joetanx/teleport/assets/90442032/22016b86-4b42-4769-9800-036b32cc6c6e)

### 5.4. Connect to Kubernetes cluster via `tsh` CLI client

> [!Note]
>
> `kubectl` should be available in `PATH` of the client machine

![image](https://github.com/joetanx/teleport/assets/90442032/b1e2c4ba-5ffe-4214-aea5-27aea04f1153)

![image](https://github.com/joetanx/teleport/assets/90442032/0e15a48e-325a-4bbe-87ce-57d1bb0cdc71)

## 6. Session recording

![image](https://github.com/joetanx/teleport/assets/90442032/d06844f7-049c-42c2-b26e-7f158b06ea35)

![image](https://github.com/joetanx/teleport/assets/90442032/5b566700-2197-42da-951c-8dd7770f760b)

![image](https://github.com/joetanx/teleport/assets/90442032/c69ea0e7-2dd4-40ca-8b35-ae8ffb25f7f1)

![image](https://github.com/joetanx/teleport/assets/90442032/eb7dab8b-a729-4116-8220-e15be13515f7)

![image](https://github.com/joetanx/teleport/assets/90442032/12e53614-9661-4489-b4c1-151deb4c587a)
