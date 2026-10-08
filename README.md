# mach-yaml

<p>
  <a href="https://github.com/briar-systems/mach-yaml/actions/workflows/ci.yml"><img src="https://github.com/briar-systems/mach-yaml/actions/workflows/ci.yml/badge.svg" alt="CI"></a>
  <a href="LICENSE"><img src="https://img.shields.io/github/license/briar-systems/mach-yaml?color=FF00FF&labelColor=000000" alt="License"></a>
</p>

**A Mach library for YAML 1.2 reading and writing.**

## Modules

- `yaml.parse` reads text into a stream of documents, each a tree of nodes. Scalars are decoded copies and every node keeps its tag, anchor and scalar style. An alias holds the node its anchor named.
- `yaml.node` is the tree: scalar, sequence, mapping and alias nodes, builders and accessors for them, `node_find`, and `node_equal`.
- `yaml.schema` reads a plain untagged scalar, or one under a core tag, as null, bool, int, float or string, without converting the tree.
- `yaml.write` writes a tree back in block style through std's writer, quoting only when the plain form would not read back the same.
- `yaml.scan` is the tokenizer under the reader.
- `yaml.error` holds the defects, each with a 1-based line and column, and the `Limits` that bound nesting depth and alias expansion.

All memory comes from the allocator you pass, and a refused allocation is an error.


## Usage

Add the dependency to `mach.toml`:

```toml
[dep.yaml]
git = "https://github.com/briar-systems/mach-yaml"
ref = "branch/dev"
```

Then read a document and release it:

```mach
use std.allocator;
use std.types.option.opt;
use std.types.result.res;
use std.types.size.usize;
use yaml;

# the tbd-version of the first document of a text stub, or -1
fun tbd_version(a: *allocator.Allocator, text: *u8, len: usize) i64 {
    val read: res[yaml.parse.Stream, yaml.error.Error] = yaml.parse.parse(text, len, a);
    if (sel read.err) {
        val at: yaml.error.Mark = yaml.error.error_mark(read.err);
        # at.line and at.column are 1-based
        ret -1;
    }
    var stream: yaml.parse.Stream = read.ok;
    fin {
        yaml.parse.dnit(?stream);
    }

    val doc: opt[*yaml.parse.Document] = yaml.parse.stream_get(?stream, 0);
    if (!sel doc.some) { ret -1; }
    val root: *yaml.node.Node = yaml.parse.document_root(doc.some);
    if (!yaml.node.node_tag_is(root, "!tapi-tbd")) { ret -1; }

    val version: opt[*yaml.node.Node] = yaml.node.node_find(root, "tbd-version");
    if (!sel version.some) { ret -1; }
    val n: opt[i64] = yaml.schema.node_int(version.some);
    if (!sel n.some) { ret -1; }
    ret n.some;
}
```


## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for building, testing and the contribution rules.


## License

[MIT](LICENSE)
