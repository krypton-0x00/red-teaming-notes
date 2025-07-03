Rsync is used for copying files locally or over a network.

PORT: 873

```sh
nc -nv 127.0.0.1 873
```
## Enumerating an Open Share

lets say above command returns a dev share.
```sh
rsync -av --list-only rsync://127.0.0.1/dev
```


```sh
rsync -av rsync://127.0.0.1/dev
```
#tip: This [guide](https://phoenixnap.com/kb/how-to-rsync-over-ssh) is helpful for understanding the syntax for using Rsync over SSH.

