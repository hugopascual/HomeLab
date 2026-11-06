# Architecture

Every service is independent: it does not need any other service to be
deployed, nor to be on the same host. All of them can run on a single host or
be spread across several ones, each host deploying only its services from its
own clone of the repository.

## Networking

There is no docker network shared between services. Every service is an
independent docker compose stack that uses its own default network and
publishes on the host the ports it needs.

The ingress (Nginx Proxy Manager) routes the traffic to `<host IP>:<published
port>`, the same way whether the service is on the same host as the proxy or
on another one. Services can be deployed in any order and moved between hosts
by only updating their proxy host entry.

Containers of the same stack (e.g. Firefly III core, database and cron) reach
each other by service name through the stack default network.

## Published ports

Web interfaces are published on these ranges:

| Type                            | HTTP          | HTTPS         |
| ------------------------------- | ------------- | ------------- |
| Network services admin panels   | `10000-10099` | `14000-14099` |
| Applications                    | `10100-...`   | `14100-...`   |

A tool that also serves HTTPS publishes it on its HTTP port plus `4000`, so it
is reachable directly without the ingress. Tools that only serve HTTP have no
HTTPS port. Ports 80 and 443 are reserved to the ingress.

| Service                        | Port         | Description                           |
| ------------------------------ | ------------ | ------------------------------------- |
| Ingress                        | 80, 443      | HTTP / HTTPS traffic                  |
| DNS                            | 53 tcp/udp   | DNS                                   |
| VPN                            | 51820 udp    | WireGuard                             |
| DNS                            | 10000, 14000 | Pi-hole admin panel (HTTP, HTTPS)     |
| VPN                            | 10001        | WGDashboard                           |
| Ingress                        | 10002        | Nginx Proxy Manager admin panel       |
| Personal Finance Manager (pfm) | 10100        | Firefly III                           |

Ports are configured in each `services/<service>/.env`.
