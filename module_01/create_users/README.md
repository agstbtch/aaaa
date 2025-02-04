# Создание локальных учетных записей

## HQ-SRV и BR-SRV

```
useradd -m -u 1010 sshuser
passwd sshuser
```

```
nano /etc/sudoers.d/sshuser
```

```
sshuser ALL=(ALL) NOPASSWD:ALL
```

### Проверка:

```
su - sshuser
sudo whoami
```

## HQ-RTR, BR-RTR (EcoRouter)

```
conf t
username net_admin
password P@ssw0rd
role admin
activate
```
