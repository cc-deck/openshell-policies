# cc-deck-catalog

Policy component catalog for [cc-deck](https://github.com/cc-deck/cc-deck) OpenShell sandbox environments.

## What This Is

This repository contains declarative YAML component files that define network policy fragments for OpenShell sandboxes.
The `cc-deck capture` command fetches these files and caches them locally.
The `cc-deck build refresh` command assembles them into a deterministic `openshell/policy.yaml`.

Updating a component here allows all cc-deck users to pick up endpoint changes without upgrading the binary.

## Structure

- `catalog.yaml` lists all available component files (consumed by `cc-deck capture`)
- `*.yaml` files are individual policy components

## Component Format

Each component declares a key, name, match conditions, and endpoints:

```yaml
key: pkg_rust
name: rust packages
match:
  tools:
    - rust
    - cargo
endpoints:
  - host: crates.io
    port: 443
  - host: index.crates.io
    port: 443
```

See the [configuration reference](https://cc-deck.github.io/cc-deck/reference/configuration.html#_custom_component_file_format) for the full schema.

## Precedence

Components from this catalog sit in the middle tier:

1. **User-local** (`.cc-deck/setup/openshell/policies/`) overrides everything
2. **Catalog** (this repo, cached in `.cc-deck/setup/openshell/components/`)
3. **Embedded** (built into the cc-deck binary) is the fallback

When the catalog and the embedded binary contain the same filename (e.g. `rust.yaml`), the catalog version wins.

## Contributing

To add or update an endpoint:

1. Edit (or create) the component YAML file
2. Update `catalog.yaml` if adding a new file
3. Open a pull request

Changes are picked up by users on their next `cc-deck capture` run.
