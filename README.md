# TiFS

A distributed file system based on TiKV.

## Run

You need a tikv cluster to run tifs. [tiup](https://github.com/pingcap/tiup) is convenient to deploy one, just install it and run `tiup playground`.

### Develop

```bash
cargo build
mkdir ~/mnt
RUST_LOG=debug target/debug/tifs --mount-point ~/mnt
```

Then you can open another shell and play with tifs in `~/mnt`.

Maybe you should enable `user_allow_other` in `/etc/fuse.conf`.

for developing under `FreeBSD`, make sure the following dependencies are met.

```bash
pkg install llvm protobuf pkgconf fusefs-libs3 cmake
```

for now, `user_allow_other` and `auto unmount` does not work for `FreeBSD`, using as `root` and manually `umount` is needed.

### Product

```bash
cargo build --features "binc" --no-default-features --release
mkdir ~/mnt
RUST_LOG=info target/release/tifs --mount-point ~/mnt
```

### Installation

```bash
cargo build --features "binc" --no-default-features --release
sudo install target/release/mount /sbin/mount.tifs
mkdir ~/mnt
mount -t tifs tifs:127.0.0.1:2379 ~/mnt
```

## FUSE
There is little docs about FUSE, refer to [example](https://github.com/cberner/fuser/blob/master/examples/simple.rs) for the meaning of FUSE API.

## TODO

- [x] FUSE API
    - [x] init
    - [x] lookup
    - [x] getattr
    - [x] setattr
    - [x] readlink
    - [x] readdir
    - [x] open
    - [x] release
    - [x] read
    - [x] write
    - [x] mkdir
    - [x] rmdir
    - [x] mknod
    - [x] lseek
    - [x] unlink
    - [x] symlink
    - [x] rename
    - [x] link
    - [x] statfs
    - [x] create
    - [x] fallocate
    - [x] getlk
    - [x] setlk

- [ ] Testing and Benchmarking
    - [x] pjdfstest
    - [ ] fio

- [ ] Other
    - [ ] real-world usage
        - [x] vim
        - [ ] emacs
        - [x] git
        - [x] gcc
        - [x] rustc
        - [ ] cargo build
        - [x] npm install
        - [x] sqlite
        - [ ] tikv on tifs
        - [x] client runs on FreeBSD: simple case works
