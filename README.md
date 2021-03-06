# node-virtualbox | [![dependencies Status](https://david-dm.org/ottomatica/node-virtualbox/status.svg)](https://david-dm.org/ottomatica/node-virtualbox)

This is a simple tool that helps provision basic VirtualBox virtual machines with sane defaults.

### Installation and Usage

Requires node >= 8.X

```
npm install node-virtualbox [--save] [-g]
```

Example run:

``` bash
node bin.js --provision --vmname "hello" --ip 172.168.0.55 --verbose
```

ssh into instance.
```
ssh -i config/resources/insecure_private_key -p 2002 -o StrictHostKeyChecking=no -o IdentitiesOnly=yes vagrant@127.0.0.1
```

### Default setup

The default VM will have 2 cpus and 1G memory. The default image is based on the latest [ubuntu/xenial64](https://cloud-images.ubuntu.com/xenial/current/) image. The VM has two NICs. The first nic uses NAT to forward incoming and outgoing traffic. The second nic is assigned a private host only network address. After creation, you can login with vagrant/vagrant or ssh with the default insecure_private_key located in `config/resources/`.

### Commands

`--list` return a list of vm names and uuids.

```
node bin.js --list
```

`--stop` Stop vm with save state.

```
node bin.js --stop --vmname <name>
```

`--delete` Unregister vm and delete all its contents.

```
node bin.js --delete --vmname <name>
```

`--info` Provide information about a vm. This will print out a json string with properties of the specified vm. 

For example, running `node .\bin.js --info --vmname "vm3"` will print the following, which can be used to retrieve properties such as the port that can be used for ssh access:

```
{ name: '"vm3"',
...
'Forwarding(0)': '"guestssh,tcp,,2002,,22"',
}
```

### Provision options

`--cpus` Set the number of cpus for VM. Default is 2 (or 1 for micro).

`--mem` Set the size of ram in MB(e.g., 512 or 1024). Default is 1024 (or 512 for micro).

`--ovf` Set the box to import when creating vm. If this is omitted, the latest ubuntu-xenial image is downloaded and used.

`--ssh_port` Set the local port used to forward ssh connections to vm. If this is omitted, then a freely available port between 2002 and 2999 is automatically assigned.

`--forward_ports` Set the port forwarding rules. Format: `"<guest_port>:<host_port>"` or `"<port>"`. `"<port>"` translates to `"<port>:<port>"`.

`--sync` Set a shared folder. Format: `"<host_folder>;<guest_folder>"`. You can provide multiple of these options.

`--add_ssh_key` Add public ssh key to ~/.ssh/authorized_keys of the vm.
```
node bin.js --provision --vmname "shared_folders_vm" --ip 172.16.1.45 --ssh_port 2095 --verbose --sync "C:\Users\chris;/chris" --sync "C:\Users\chris\projects;/projects" --add_ssh_key ~/.ssh/id_rsa.pub
```

### Micro VM
This is for booting micro kernels (custom initramfs inside of an iso)

Example micro VMs (iso):
``` bash
node bin.js --micro --vmname "micro" --attach_iso <Path>
```

If you don't specify path to an iso it will download and use an Alpine linux iso.
