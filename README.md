# ydotool

Generic Linux command-line automation tool

+ keyd configuration for Helldivers II stratagems `etc/keyd/hellpad.conf`

+ bash script that generates the sequences `hellpad_ctrlseq`

# How this works

- keyd listens to key strokes from specific device, as defined in `etc/keyd/hellpad.conf`
- triggers the script `hellpad_ctrlseq` for bound keys with the appropriate sequence
- which then calls `ydotool` which then triggers the `ydotoold` service to inject the stratagem keystrokes

# How to set this up

- be on Linux
- install `keyd` from distro repo
- copy example `etc/keyd/hellpad.conf` from this repo to your `etc/keyd/`
- get your macro keyboard device ID by pressing some keys (device ID will look like this: 258a:1006:e656605b)
```
keyd.rvaiya monitor
```
- edit `etc/keyd/hellpad.conf` and replace placeholder with your device ID
- restart keyd and look at syslog for errors
```
systemctl restart keyd
journalctl -n 1000 | grep keyd
```
- compile `ydotool` (distro repo one probably doesn't work, gotta do this one manually)
```
# install prerequisite
apt install scdoc

# build ydotool
mkdir build
cd build
cmake ..
make -j `nproc`
make install
cd ..

# install as a service:
./install-service

# verify service is running:
systemctl status ydotoold
```

- look at the log while you press keys to verify it's working
```
tail -f /var/log/hellpad.log
```
