# k8s-the-harder-way

> [!IMPORTANT]
> Works only for **macOS/ARM64**

This repo is only for my personal use. I want to understand how Kubernetes works behind the hood by following the [Kubernetes The Harder Way](https://github.com/ghik/kubernetes-the-harder-way)

## Where I have stopped

I have stopped at the [Bootstrapping Kubernetes Security](https://github.com/ghik/kubernetes-the-harder-way/blob/main/docs/04_Bootstrapping_Kubernetes_Security.md#bootstrapping-kubernetes-security) section.

## Utils

### Setup commands

An abstract numeric ID is assigned to VMs. This ID will come in handy in scripts when we want to differentiate between the VMs:

- `gateway` has ID 0
- `control` nodes have IDs 1 through 3
- `worker` nodes have IDs 4 through 6

- Setup the vm
   ```bash
    for vmid in $(seq 0 6); do
      ./vmsetup.sh $vmid
    done
    ```
- Setup vm SSH
   ```bash
    ./vmssh.sh $vmid
    ```
- Run a VM
  ```bash
  sudo qemu-system-aarch64 \
    -nographic \
    -machine virt,accel=hvf,highmem=on \
    -cpu host \
    -smp 2 \
    -m 2G \
    -bios /opt/homebrew/share/qemu/edk2-aarch64-code.fd \
    -nic vmnet-shared,start-address=192.168.1.1,end-address=192.168.1.20,subnet-mask=255.255.255.0,mac=52:52:52:00:00:00 \
    -hda gateway/disk.img \
    -drive file=gateway/cidata.iso,driver=raw,if=virtio
  ```

### Launch VMs

- `sudo ./vmlaunchall.sh kubenet-qemu`
- `sudo tmux attach -t kubenet-qemu`
- `sudo tmux kill-session -t kubenet-qemu`
- `./vmsshall.sh kubenet-ssh`

### QEMU utils

- `Ctrl`+`Opt`+`2` => Switch to serial port
- `Ctrl`+`Opt`+`3` => Switch to parallel port
- `Ctrl`+`Opt`+`2` => Switch to monitor console
- `Ctrl`+`Opt`+`G` => release mouse

### `tmux`

- https://tmuxcheatsheet.com/
