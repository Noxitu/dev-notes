# Management

Control service:

    systemctl [status|start|stop|restart] <service_name>

Service logs (`-f` is for follow):

    journalctl -u <service_name> -f


# Configuration

Services require `.service` sufix in name. Configuration is located in `/etc/systemd/system/`.

## Basic service requiring manual start

    [Unit]
    Description=<description>

    [Service]
    Type=simple
    User=<username>
    Group=<username>
    ExecStart=<executable>
    WorkingDirectory=<working_directory>
    StandardInput=null
    StandardOutput=journal
    StandardError=journal
    Restart=no

## Auto start and restart

    [Unit]
    After=multi-user.target

    [Service]
    Restart=on-failure
    RestartSec=5

    [Install]
    WantedBy=multi-user.target

## Socket for use as stdin

    [Service]
    Sockets=<systemd_socket_name>.socket
    StandardInput=socket

Requires additional socket configuration in `<systemd_socket_name>.socket` file:

    [Socket]
    ListenFIFO=<unix_pipe_path>
    Service=<service_name>.service

Sockets are also capable of starting service on new data.

## Timers

Monotonic timer in `<timer_name>.timer`:

    [Unit]
    Description=<description>

    [Timer]
    OnBootSec=15min
    OnUnitActiveSec=15min

    [Install]
    WantedBy=timers.target

With an appropieate `<timer_name>.service`:

    [Unit]
    Description=<description>

    [Service]
    Type=oneshot
    ExecStart=<app>
    User=<user>
    Group=<group>

# Other Examples

## Example status

    $ systemctl status minecraftd
    ● minecraftd.service - Minecraft Server
    Loaded: loaded (/etc/systemd/system/minecraftd.service; enabled; vendor preset: disabled)
    Active: active (running) since Sun 2021-12-26 18:00:47 GMT; 4 months 4 days ago
    Main PID: 22059 (sh)
    CGroup: /system.slice/minecraftd.service
            ├─22059 /bin/sh /home/minecraft/run.sh
            └─22060 java -Xmx3072M -Xms3072M -Dlog4j2.formatMsgNoLookups=true -jar /home/minecraft/jars/minecraft_server.1.18.1.jar nogui

    <few log lines>