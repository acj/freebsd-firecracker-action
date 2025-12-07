# Run FreeBSD in a Firecracker VM

This GitHub Action launches a Firecracker VM running FreeBSD. Typical boot time is 10-15 seconds.

## Getting started

```yaml
- name: Launch Firecracker VM
  uses: acj/freebsd-firecracker-action@v0.6.0
  with:
    run-in-vm: |
      echo "Hello from inside the VM!"
```

## How it works

This action uses a FreeBSD kernel, rootfs, and Firecracker binary from [freebsd-firecracker](https://github.com/acj/freebsd-firecracker). These include patches to make FreeBSD boot in Firecracker on the GitHub Actions runner hardware.

## Current status

- [X] Boots a VM in \~12 seconds in GitHub Actions
- [X] Supports FreeBSD 14.3-STABLE and Firecracker 1.13.0
- [X] Supports Intel and AMD CPUs

## Supported inputs

### `pre-run`: Run commands after the VM starts

With the `pre-run` input, you can run commands _outside_ of the VM after the VM is up and running. This is useful for copying files into the VM, for example.

```yaml
- name: Launch Firecracker VM
  uses: acj/freebsd-firecracker-action@v0.6.0
  with:
    pre-run: |
      echo "Hello from outside the VM!"
```

### `run-in-vm`: Run commands inside the VM

With the `run-in-vm` input, you can run commands _inside_ of the VM.

```yaml
- name: Launch Firecracker VM
  uses: acj/freebsd-firecracker-action@v0.6.0
  with:
    run-in-vm: |
      echo "Hello from inside the VM!"
```

### `post-run`: Run commands outside the VM before it shuts down

With the `post-run` input, you can run commands _outside_ of the VM before the VM shuts down. This is useful for copying files out of the VM.

```yaml
- name: Launch Firecracker VM
  uses: acj/freebsd-firecracker-action@v0.6.0
  with:
    post-run: |
      echo "Hello from outside the VM!"
```

### `checkout`

Default: `true`

Clone the repository into the VM. You can disable this behavior by setting `checkout` to `false`, which will disable the `actions/checkout` child action.

### `disk-size`

Default: `2G`

The size of the VM's disk image in units recognized by [truncate(1)](https://linux.die.net/man/1/truncate):

> Units are K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,... (powers of 1000). Binary prefixes can be used, too: KiB=K, MiB=M, and so on.

This can be increased if you need more space inside the VM for larger repositories, builds that produce large output, etc.

### `continue-on-error`

Default: `false`

Whether to continue if the `run` script returns a non-zero exit code. Setting this to `true` will guarantee that the `post-run` script is run.

### `verbose`

Default: `false`

Prints extra information about the runner and the VM. Useful for debugging.

### Less commonly used inputs

These inputs are provided for debugging and testing. You probably don't need to use them.

#### `kernel-url`

Overrides the URL where the FreeBSD kernel image is downloaded from.

#### `rootfs-url`

Overrides the URL where the FreeBSD rootfs image is downloaded from.

#### `firecracker-url`

Overrides the URL where the Firecracker binary is downloaded from.

#### `ssh-private-key-url`

Overrides the URL where the SSH private key for the VM is downloaded from.

#### `ssh-public-key-url`

Overrides the URL where the SSH public key for the VM is downloaded from.

## Copying files to/from the VM

By default, we use `rsync` to copy the current directory into the VM. A few items like the `.git` directory are excluded. You can customize this behavior by overriding the `pre-run` and `post-run` inputs. See the defaults in `action.yml` for a useful starting point.

## Contributing

Please be kind. We're all trying to do our best.

If you're having trouble, open an issue. If you'd like to suggest an improvement, open a PR. For bigger changes, please open an issue first so that we can discuss the idea.

## License

Apache 2.0
