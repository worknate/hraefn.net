date: 2003-06-19 06:00:00
title: Setting up Keyed SSH Connections
tags: ops
---

These are notes for creating a one-way connection.  For our purposes, the machine you are connecting *from* is referred to as the "local" machine, and the machine you are connecting *to* is referred to as the "remote" machine.

These steps work fine even if the usernames on the "local" and "remote" machines are different.

#### Generate keys on local

1. Change to the ~/.ssh directory on the "local" machine  
2. Run `ssh-keygen -t dsa` once  
3. This should make two files, a public key `id_dsa.pub` and a private key `id_dsa`.  
4. Then make a copy of the public key with the "local" hostname, for example:  

		$ cp id_dsa.pub dev.id_dsa.pub

#### Move public key to remote

1. Copy the "local" public key to the "remote" ~/.ssh directory, one way or another. For example:

		$ scp dev.id_dsa.pub nstilwell@test:~/.ssh

3. Now you see why we renamed the key?  So as not to overwrite the "remote" machine`s public key, so we don`t get confused ;)  I am unsure how important this is.

#### Make/update authorized_keys2 on remote

Append the public key from "local" to the end of ~/.ssh/authorized_keys on "remote". For example:

		$ cat dev.id_dsa.pub >> authorized_keys2

#### Set permissions

1. The ~/.ssh directories on both machines should be available (drwxr-xr-x):

		$ chmod 755 ~/.ssh

2. Private keys and the authorized_keys2 file need to be private (-rw------):

		"local"   ->  $ chmod 600 ~/.ssh/id_dsa
		"remote"  ->  $ chmod 600 ~/.ssh/authorized_keys2