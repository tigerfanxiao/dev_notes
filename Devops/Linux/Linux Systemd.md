
```shell
cd /etc/systemd/system

```


custom service
```bash
[Unit] 
Description=Custom 
Service Wants=network-online.target 
After=network-online.target

[Service] 
Type=forking 
ExecStart=/usr/bin/emacs --daemon 
ExecStop=/usr/bin/emacsclient --eval "(kill-emacs)" 

[Install] 
WantedBy=default.target
```


```shell
systemctl start custom.service
systemctl status custom.service
```