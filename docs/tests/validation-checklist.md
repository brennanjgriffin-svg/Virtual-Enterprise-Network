# Validation Checklist

## Network
- [ ] Ping GW from each host
- [ ] DNS resolves `dc-01.lab.local`, `www.lab.local`

## AD
- [ ] Join Windows client to domain
- [ ] GPO applies (check `gpresult`)

## Services
- [ ] SMB share mounts from Win & Linux
- [ ] Web page loads at `http://www.lab.local`
- [ ] Backup job runs (see logs), restore test passes

## Monitoring
- [ ] All targets UP
- [ ] Alerts fire on simulated outage
