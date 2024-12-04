# Run FreeBSD in a Firecracker VM

This GitHub Action launches a Firecracker VM running FreeBSD. Typical boot time is 10-15 seconds.

## Getting started

```yaml
- name: Launch Firecracker VM
  uses: acj/freebsd-firecracker-action@v0.1.1
  with:
    run: |
      echo "Hello from inside the VM!"
```

## How it works

This action uses a FreeBSD kernel, rootfs, and Firecracker binary from [freebsd-firecracker](https://github.com/acj/freebsd-firecracker). These include patches to make FreeBSD boot in Firecracker on the GitHub Actions runner hardware.

## Current status

- [X] Boots a VM in \~12 seconds in GitHub Actions
- [X] Supports FreeBSD 14.0-RELEASE
- [X] Supports Intel and AMD CPUs

## Supported inputs

### `pre-run`: Run commands after the VM starts

With the `pre-run` input, you can run commands _outside_ of the VM after the VM is up and running. This is useful for copying files into the VM, for example.

```yaml
- name: Launch Firecracker VM
  uses: acj/freebsd-firecracker-action@v0.1.1
  with:
    pre-run: |
      echo "Hello from outside the VM!"
```

### `run`: Run commands inside the VM

With the `run` input, you can run commands _inside_ of the VM.

```yaml
- name: Launch Firecracker VM
  uses: acj/freebsd-firecracker-action@v0.1.1
  with:
    run: |
      echo "Hello from inside the VM!"
```

### `post-run`: Run commands outside the VM before it shuts down

With the `post-run` input, you can run commands _outside_ of the VM before the VM shuts down. This is useful for copying files out of the VM.

```yaml
- name: Launch Firecracker VM
  uses: acj/freebsd-firecracker-action@v0.1.1
  with:
    post-run: |
      echo "Hello from outside the VM!"
```

### `checkout`

By default, the action will clone the repository into the VM. You can disable this behavior by setting `checkout` to `false`, which will disable the `actions/checkout` child action.

### `continue-on-error`

By default, the action will abort if the `run` script returns a non-zero exit code. You can override this behavior by setting `continue-on-error` to `true`, which will guarantee that the `post-run` script is run.

### `verbose`

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
