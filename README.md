# mach-yaml

<p>
  <a href="https://github.com/briar-systems/mach-yaml/actions/workflows/ci.yml"><img src="https://github.com/briar-systems/mach-yaml/actions/workflows/ci.yml/badge.svg" alt="CI"></a>
  <a href="LICENSE"><img src="https://img.shields.io/github/license/briar-systems/mach-yaml?color=FF00FF&labelColor=000000" alt="License"></a>
</p>

**A Mach library for YAML 1.2 reading and writing.**

## Usage

Add the dependency to `mach.toml`:

```toml
[dep.yaml]
git = "https://github.com/briar-systems/mach-yaml"
version = "^0.1"
```

Then bind the library in a source file:

```mach
use yaml;
```


## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for building, testing and the contribution rules.


## License

[MIT](LICENSE)
