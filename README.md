### HOWTO

- create a VM
- attach disk(s) with existing OSes
- boot e.g. daily canary
- enable `deb-src` for `main` in `/etc/apt/sources.list`
- install deps
  ```sh
  sudo apt install -y build-essential git libnl-*-dev libpython3-dev pkg-config python3-setuptools python3-pyudev
  ```
- get probert
  ```sh
  git clone https://github.com/canonical/probert
  cd probert
  ```
- patch probert
  ```diff
  diff --git a/probert/storage.py b/probert/storage.py
  index 33a3068..d6aa63d 100644
  --- a/probert/storage.py
  +++ b/probert/storage.py
  @@ -168,8 +168,8 @@ class Storage():
          'lvm': Probe(lvm.probe),
          'mount': Probe(mount.probe),
          'multipath': Probe(multipath.probe),
  -        'os': Probe(os.probe, in_default_set=False),
  -        'filesystem_sizing': Probe(null_probe, in_default_set=False),
  +        'os': Probe(os.probe, in_default_set=True),
  +        'filesystem_sizing': Probe(null_probe, in_default_set=True),
          'raid': Probe(raid.probe),
          'zfs': Probe(zfs.probe),
      }
  ```
- build probert
  ```sh
  python3 setup.py build_ext -i
  ```
- probe all
  ```sh
  sudo -E env PYTHONPATH=$PWD ./bin/probert --all
  ```
