## systemctl podman template

[Unit]
Description=Mailserver
Requires=syslog.service

[Service]
Restart=always
ExecStart=/usr/bin/podman start -a mailserver1
ExecStop=/usr/bin/podman stop -t 2 mailserver1

[Install]
RequiredBy=multi-user.target