# JeriCommerce Claude Code Plugins

Official plugin marketplace for [Claude Code](https://code.claude.com) by JeriCommerce.

## Installation

Add this marketplace to your Claude Code:

```
/plugin marketplace add JeriCommerce/claude-plugins
```

Then install individual plugins:

```
/plugin install loyalty-specialist@jericommerce-plugins
```

## Available Plugins

| Plugin | Description | Version |
|--------|-------------|---------|
| [loyalty-specialist](./plugins/loyalty-specialist/) | Generate professional PDF loyalty program performance reports by querying Metabase and analyzing program data | 0.1.0 |

## Updating

To get the latest plugins:

```
/plugin marketplace update
```

## Contributing

To add a new plugin:

1. Create a directory under `plugins/your-plugin-name/`
2. Add `.claude-plugin/plugin.json` with plugin metadata
3. Add your skills, commands, agents, or hooks
4. Register the plugin in `.claude-plugin/marketplace.json`
5. Submit a PR

## License

Proprietary — JeriCommerce
