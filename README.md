Running a zkBOB relayer
----

This document describes how to run the zkBOB relayer

## Assumptions

- This document assumes that you already have fawkes-compatible parameters for ZKP (for transfer and tree updates).
- You have a persistent Linux machine with docker installed on it, which is accessible from the public internet via a fixed IP address. We recommend using a system with at least 4 CPU, 8 GB RAM, 100 GB SSD.

## Configuration

1) Create an empty directory somewhere on the VM (e.g. `/opt/zk`)
2) Clone the repository with all necessary configs:
```bash
cd /opt/zk
git clone https://github.com/zkBob/relayer-launch.git .
```
3) Create `.env` file from the example at `.env.example` (for the staging `.env.sepolia` can be used). Put the the pool contract address and the relayer private key in the config. Other values can be left without changes.
4) Put ZKP parameters to the `params` directory.

If it is required to send the relayer logs to a remote syslog server the following actions can be done (it is assumed that the `docker-compose.yml` symlink points to the `docker-compose-syslog.yml` file).

5) Copy `./syslog/etc/logrotate.d/docker-logs` to `/etc/logrotate.d/`.
6) Copy `./syslog/etc/rsyslog.d/30-zkbob-local.conf` and `./syslog/etc/rsyslog.d/35-zkbob-remote-logging.conf` to `/etc/rsyslog.d/`.
7) Modify `target` and `port` in `/etc/rsyslog.d/35-zkbob-remote-logging.conf` to point to a remote syslog server.
8) Restart the rsyslog service by `systemctl restart syslog`.

## Running node

Run the following command to start the zkBOB relayer:

```bash
docker compose up -d
```

To monitor he process of the relayer's boot up the logs can be accessible by

```
docker compose logs -f zkbob-relayer
```
