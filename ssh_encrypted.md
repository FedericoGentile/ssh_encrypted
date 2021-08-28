# SSH connection and key pairing
These are the steps to apply to the local and remote computer in order to access via ssh protocol to the remote computer without using a password and using key encryption pairing to secure the connection.

## Step 1
Somehow get the IP address of the Raspberry Pi. It should be something like this: `192.168.1.52`

## Step 2

Open a shell and access the Raspberry Pi via ssh: 
```
ssh pi@192.168.1.52
```
You will need the password.

## Step 3
In the home directory of the remote pc use these commands:
```
mkdir .ssh
```

## Step 4
Secure the ssh connection via private/public key.
In the local pc use this commands:
```
ssh-keygen -f .ssh/fede_windows -t rsa -b 4096
```
If your local machine is Linux based run this line:
```
chmod 600 .ssh/fede_windows # if linux
```
Finally:
```
scp .ssh/fede_windows.pub pi@192.168.1.52:.ssh
```

## Step 5
In the remote pc use these commands:
```
sudo nano /etc/ssh/sshd_config
```
and modify the following lines of the config file:
```
ChallengeResponseAuthentication no
PasswordAuthentication no
UsePAM no
```
Finally:
```
sudo systemctl reload sshd
```
## Step 6
In the remote computer use these commands:
```
cat ~/.ssh/fede_windows.pub >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh/
chmod 600 ~/.ssh/*
```

## Step 7
In the local computer run this command to log in to the remote one:
```
ssh -i .ssh/fede_windows pi@192.168.1.52
```

## Step 8
If your local coputer is Linux based and you want to avoid ssh-agent from using previously working keys, then used this command:
```
sudo nano /etc/ssh/ssh_config 
```
and add the following line of code under *Host **:
```
  IdentityAgent none
```

