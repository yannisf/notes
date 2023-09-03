# ERX

## Update `config.boot` manually

1. Copy `/config/config.boot` to e.g. `/config/updated.config.boot`
2. Perform any changes you might want to `/config/updated.config.boot`
3. Load, review and persist the updated configuration like so:
   1. `configure`
   1. `load updated.config.boot`
   1. `compare`
   1. `commit`
   1. `save`
   1. `exit`

## Wireguard and firmware upgrades

When EdgeOS updates its firmware it loses the wireguard installation and any wireguard configuration that might be `disabled`. Thus, before updating, ensure all wireguard configuration is enabled, or else you will have to restore it manually from previous backups.



