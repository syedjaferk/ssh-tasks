### Awesome Blogs:

1. https://www.marcobehler.com/guides/ssh-commands


### Dockerfile for SSH

1. Building docker image: `sudo docker build -t my_ssh_image .`
2. Running the container of image: `sudo docker run -d -p 2222:22 --name my_ssh_container my_ssh_image`
3. Find the ip of the container: `sudo docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' my_ssh_container`
4. ssh to the container: `ssh root@<ip of the above>`

### About OpenSSH

- OpenSSH is a remote management tool, that gives you access to run commands on another machine. 
- Developed by the OpenBSD project.
- It's the closest thing to a standard for remote access we have in the Linux community.
- It's a suite of utilities, the most important of which are the server and client components. 

1. Where its installed ? `which ssh`
2. To check whether its installed ? `apt search openssh-client`

### To connect to a server 

- When we connect to a server for the first time, it will ask for the authenticity

```
[10:32 AM]-[syedjafer@syedjafer]-[~/.../Projects/ssh-tasks]- |master U:2 ✗|
$ ssh root@172.17.0.2
The authenticity of host '172.17.0.2 (172.17.0.2)' can't be established.
ED25519 key fingerprint is SHA256:PphbDMTybQEyHoXJxBVokgHfuL1a+acHtpCZa4VXur8.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.17.0.2' (ED25519) to the list of known hosts.
root@172.17.0.2's password: 
Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 6.5.0-27-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

```


### Known Hosts


### How to generate keys - SSH-KEYGEN

`ssh-keygen -t rsa -b 4096 -C "syedjafer1997@gmail.com"`

The -C file simply puts a comment on your public key, like below, so you can e.g. easily make out which public key belongs to which email address, in a busy Authorized_Keys file.

> Note: When generating SSH keys, make sure to protect your private key with a passphrase.

By default, the keys will be stored inside `/home/<username>/.ssh/`
```
[03:18 PM]-[syedjafer@syedjafer]-[~/.ssh]-
$ ls /home/syedjafer/.ssh/
id_rsa  id_rsa.pub  known_hosts  known_hosts.old

```

If needed we can specify the path, where it should be created. 

```
[03:29 PM]-[syedjafer@syedjafer]-[~/.../Projects/ssh-tasks]- |master U:2 ✗|
$ ssh-keygen -t rsa -b 4096 -C "syedjafer1997@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/syedjafer/.ssh/id_rsa): test_file_ssh
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in test_file_ssh
Your public key has been saved in test_file_ssh.pub
The key fingerprint is:
SHA256:7SRFKQpbTrO4RmIVWSejalFPOFYLm5UJcLMCD4n+VoA syedjafer1997@gmail.com
The key's randomart image is:
+---[RSA 4096]----+
|+.o.BBBo. ..     |
|oE +B@*=...      |
|. +o*Oo+ ..      |
| .o++.+  o       |
| .+o..  S o      |
| . oo    +       |
|  ..      .      |
|                 |
|                 |
+----[SHA256]-----+

[03:30 PM]-[syedjafer@syedjafer]-[~/.../Projects/ssh-tasks]- |master U:2 ?:2 ✗|
$ ls
Dockerfile  ssh.notes  ssh_notes.md  test_file_ssh  test_file_ssh.pub

[03:30 PM]-[syedjafer@syedjafer]-[~/.../Projects/ssh-tasks]- |master U:2 ?:2 ✗|
$ 

```

### SSH with keys

To use a specific private key to connect to a server, use: 

`ssh -i <keyfile> <username>@<ip-of-machine>`

### Authorized Keys

Any remote host or service, like GitHub, that you want to use your SSH keys with, needs the public key of your SSH keypair.

For servers, you simply need to append your public key to the file ~/.ssh/authorized_keys.

Use one of the following commands to do that:

    cat ~/.ssh/id_rsa.pub | ssh USER@HOST "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys" - https://askubuntu.com/a/262074

    ssh-copy-id user@host - https://askubuntu.com/a/46427

With services like GitHub, AWS etc. you would either use the UI to upload your public key, or, if available, command-line tools.


### SCP 

1. To upload a file to a remote server: `scp myfile.txt user@ip:/dest/path`
2. To recursively upload a local folder to a remote server: `scp -rp sourcedirectory user@dest:/path`
3. To download a file from a remote server: `scp user@dest:/path/myfile.txt localpath`
4. To recursively download a local folder to a remote server: `scp -rp user@dest:/remotedir localpath`

### SSH Config

Create a file `~/.ssh/config` to manage your SSH hosts. Example:

```
Host dev-meta*
    User ec2-user
    IdentityFile ~/.ssh/johnsnow.pem

Host dev-meta-facebook
    Hostname 192.168.178.1

Host dev-meta-whatsapp
    Hostname 192.168.178.2

Host api.google.com
    User googleUser
    IdentityFile ~/.ssh/targaryen.key
```



With the config file above, you could do a:

```bash
ssh dev-meta-facebook
```

Which would effectively do a ssh -i ~/.ssh/johnsnow.pem ec2-user@192.168.178.1 for you.



### To connect to a different machine

Make sure ssh-server is installed in that machine and enable the firewall. (i.e) open port 22. 
