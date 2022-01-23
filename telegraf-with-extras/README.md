# Telegraf /w Extras
A simple [Telegraf](https://docs.influxdata.com/telegraf/v1.21/) image baked with some additional [external plugins](https://github.com/influxdata/telegraf/blob/master/docs/EXTERNAL_PLUGINS.md) plugins and some extra packages to work with built-in plugins


## Extra Plugins
### Inputs
- [plex](https://github.com/russorat/telegraf-webhooks-plex) - Listens for events from Plex Media Server [Webhooks](https://support.plex.tv/articles/115002267687-webhooks/).

### Processors
- [geoip](https://github.com/a-bali/telegraf-geoip) - Add GeoIP information to IP addresses.

## Extra Packages
- [smartmontools](https://www.smartmontools.org/) for use with the [SMART](https://github.com/influxdata/telegraf/blob/master/plugins/inputs/smart/README.md) plugin