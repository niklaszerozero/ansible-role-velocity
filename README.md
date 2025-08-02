# Ansible Role: Velocity

[![CI](https://github.com/niklaszerozero/ansible-role-velocity/actions/workflows/ci.yml/badge.svg)](https://github.com/niklaszerozero/ansible-role-velocity/actions/workflows/ci.yml)

Installs the [Velocity Minecraft Proxy](https://velocitypowered.com/).

After the role is run, a `velocity` service will be available on the server. You can use `service velocity [start|stop|restart|status]` to control the proxy.

## Requirements

Java 17 or higher must be installed.
You can install it using the [geerlingguy.java](https://github.com/geerlingguy/ansible-role-java) role or your own method.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
velocity_version: 3.4.0-SNAPSHOT
```

The version of Velocity to install.

```yaml
velocity_path: /opt/velocity
```

The installation directory for Velocity.

```yaml
velocity_group: velocity
```

The system group under which Velocity will run.

```yaml
velocity_user: velocity
```

The system user under which Velocity will run.

### Java

```yaml
velocity_java_maximum_memory: 2048M
```

The maximum Java heap size allocated to Velocity.

```yaml
velocity_java_initial_memory: 2048M
```

The initial Java heap size allocated to Velocity.

### Velocity: General Settings

```yaml
velocity_config: velocity.toml
```

The name of the Velocity configuration file.

```yaml
velocity_bind_addr: 0.0.0.0:25565
```

The IP address and port Velocity will listen on.

```yaml
velocity_motd: <#09add3>A Velocity Server
```

The server MOTD (Message of the Day) shown to players.

```yaml
velocity_show_max_players: 500
```

The number of maximum players displayed in the server list.

```yaml
velocity_online_mode: true
```

Whether Velocity should enforce online mode (player authentication with Mojang/Microsoft).

```yaml
velocity_force_key_authentication: true
```

Forces Velocity to verify player keys to ensure connection security.

```yaml
velocity_prevent_client_proxy_connections: false
```

Prevents players from connecting using other proxies.

```yaml
velocity_player_info_forwarding_mode: "NONE"
```

The method used to forward player information (e.g., `NONE`, `BUNGEEGUARD`, `MODERN`).

```yaml
velocity_forwarding_secret: ""
```

The secret key used for player information forwarding (if enabled).

```yaml
velocity_announce_forge: false
```

Enables or disables announcing Forge support to clients.

```yaml
velocity_kick_existing_players: false
```

If true, existing players will be kicked if they log in again.

```yaml
velocity_ping_passthrough: "DISABLED"
```

Whether server ping requests should be passed through to a backend server.

```yaml
velocity_sample_players_in_ping: false
```

If true, Velocity will sample players in the server list ping.

```yaml
velocity_enable_player_address_logging: true
```

If true, Velocity will log the IP addresses of players connecting.

```yaml
velocity_servers:
  - name: lobby
    addr: 127.0.0.1:30066
  - name: factions
    addr: 127.0.0.1:30067
  - name: minigames
    addr: 127.0.0.1:30068
```

The list of backend servers available to Velocity.

```yaml
velocity_try:
  - lobby
```

The list of servers Velocity should attempt to connect players to by default.

```yaml
velocity_forced_hosts:
  lobby.example.com:
    - lobby
  factions.example.com:
    - factions
  minigames.example.com:
    - minigames
```

Mapping of hostnames to backend servers.

### Velocity: Advanced Settings

```yaml
velocity_advanced_compression_threshold: 256
```

The threshold (in bytes) for compressing packets.

```yaml
velocity_advanced_compression_level: -1
```

Compression level for packets (`-1` uses the default).

```yaml
velocity_advanced_login_ratelimit: 3000
```

Rate limit for login attempts (in milliseconds).

```yaml
velocity_advanced_connection_timeout: 5000
```

Timeout for client connections (in milliseconds).

```yaml
velocity_advanced_read_timeout: 30000
```

Timeout for reading data from clients (in milliseconds).

```yaml
velocity_advanced_haproxy_protocol: false
```

Enables or disables HAProxy protocol support.

```yaml
velocity_advanced_tcp_fast_open: false
```

Enables or disables TCP Fast Open.

```yaml
velocity_advanced_bungee_plugin_message_channel: true
```

If true, Velocity will use the BungeeCord plugin messaging channel.

```yaml
velocity_advanced_show_ping_requests: false
```

If true, logs all ping requests for debugging.

```yaml
velocity_advanced_failover_on_unexpected_server_disconnect: true
```

If true, players are moved to another server if their current server disconnects unexpectedly.

```yaml
velocity_advanced_announce_proxy_commands: true
```

If true, proxy commands are announced to the connected servers.

```yaml
velocity_advanced_log_command_executions: false
```

If true, executed commands are logged.

```yaml
velocity_advanced_log_player_connections: true
```

If true, player connections are logged.

```yaml
velocity_advanced_accepts_transfers: false
```

Enables support for plugin-based server transfers.

```yaml
velocity_advanced_enable_reuse_port: false
```

If true, enables `SO_REUSEPORT` for improved performance.

```yaml
velocity_advanced_command_rate_limit: 50
```

Maximum allowed commands per second before rate-limiting applies.

```yaml
velocity_advanced_forward_commands_if_rate_limited: true
```

If true, rate-limited commands will still be forwarded.

```yaml
velocity_advanced_kick_after_rate_limited_commands: 0
```

Number of rate-limited commands before a player is kicked. `0` disables kicking.

```yaml
velocity_advanced_tab_complete_rate_limit: 10
```

Maximum allowed tab completions per second before rate-limiting applies.

```yaml
velocity_advanced_kick_after_rate_limited_tab_completes: 0
```

Number of rate-limited tab completions before a player is kicked. `0` disables kicking.

### Velocity: Query Settings

```yaml
velocity_query_enabled: false
```

Enables or disables the server query protocol.

```yaml
velocity_query_port: 25565
```

The port for query requests.

```yaml
velocity_query_map: "Velocity"
```

The map name returned by query requests.

```yaml
velocity_query_show_plugins: false
```

If true, query responses will include the list of installed plugins.

### Velocity: Plugins

```yaml
velocity_plugins_src: plugins/
```

Directory containing plugins to be deployed to Velocity.

## Dependencies

None.

## Example Playbook

```yaml
- hosts: server
  vars_files:
    - vars/main.yml
  roles:
    - role: geerlingguy.java
    - role: niklaszerozero.velocity
```

_Inside `vars/main.yml`_:

```yaml
velocity_bind_addr: 0.0.0.0:25565
velocity_motd: "<#09add3>My Custom Proxy"
velocity_servers:
  - name: lobby
    addr: 192.168.1.100:30066
velocity_try:
  - lobby
velocity_forced_hosts:
  proxy.example.com:
    - lobby
```

## License

MIT

## Author Information

This role was created in 2025 by [Niklas Vlach](https://www.niklas-vlach.com/).
