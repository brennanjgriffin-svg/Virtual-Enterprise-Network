# Service Catalog

| Host   | Role                     | Key Software         | Config Highlights                      |
|--------|--------------------------|----------------------|----------------------------------------|
| srv-01 | File/Web/Backup/SSH     | Samba, Nginx, rsync  | Shares: /srv/shares, vhosts: www.lab.local |
| mon-01 | Monitoring               | Prometheus, Grafana  | Scrape: DC, srv-01, OPNsense exporters |

See: [web.md](web.md) · [file-sharing.md](file-sharing.md) · [backups.md](backups.md)
