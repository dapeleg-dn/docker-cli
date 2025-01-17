# plugin ls

<!---MARKER_GEN_START-->
List plugins

### Aliases

`docker plugin ls`, `docker plugin list`

### Options

| Name                                   | Type     | Default | Description                                                                                                                                                                                                                                                                                                                                                                                                                          |
|:---------------------------------------|:---------|:--------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [`-f`](#filter), [`--filter`](#filter) | `filter` |         | Provide filter values (e.g. `enabled=true`)                                                                                                                                                                                                                                                                                                                                                                                          |
| [`--format`](#format)                  | `string` |         | Format output using a custom template:<br>'table':            Print output in table format with column headers (default)<br>'table TEMPLATE':   Print output in table format using the given Go template<br>'json':             Print in JSON format<br>'TEMPLATE':         Print output using the given Go template.<br>Refer to [Formatting](https://docs.docker.com/go/formatting/) for more information about formatting output with templates |
| `--no-trunc`                           |          |         | Don't truncate output                                                                                                                                                                                                                                                                                                                                                                                                                |
| `-q`, `--quiet`                        |          |         | Only display plugin IDs                                                                                                                                                                                                                                                                                                                                                                                                              |


<!---MARKER_GEN_END-->

## Description

Lists all the plugins that are currently installed. You can install plugins
using the [`docker plugin install`](plugin_install.md) command.
You can also filter using the `-f` or `--filter` flag.
Refer to the [filtering](#filter) section for more information about available filter options.

## Examples

```console
$ docker plugin ls

ID            NAME                                    DESCRIPTION                ENABLED
69553ca1d123  tiborvass/sample-volume-plugin:latest   A test plugin for Docker   true
```

### <a name="filter"></a> Filtering (--filter)

The filtering flag (`-f` or `--filter`) format is of "key=value". If there is more
than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`)

The currently supported filters are:

* enabled (boolean - true or false, 0 or 1)
* capability (string - currently `volumedriver`, `networkdriver`, `ipamdriver`, `logdriver`, `metricscollector`, or `authz`)

#### enabled

The `enabled` filter matches on plugins enabled or disabled.

#### capability

The `capability` filter matches on plugin capabilities. One plugin
might have multiple capabilities. Currently `volumedriver`, `networkdriver`,
`ipamdriver`, `logdriver`, `metricscollector`, and `authz` are supported capabilities.

```console
$ docker plugin install --disable vieux/sshfs

Installed plugin vieux/sshfs

$ docker plugin ls --filter enabled=true

ID                  NAME                DESCRIPTION         ENABLED
```

### <a name="format"></a> Format the output (--format)

The formatting options (`--format`) pretty-prints plugins output
using a Go template.

Valid placeholders for the Go template are listed below:

| Placeholder        | Description                                     |
|--------------------|-------------------------------------------------|
| `.ID`              | Plugin ID                                       |
| `.Name`            | Plugin name and tag                             |
| `.Description`     | Plugin description                              |
| `.Enabled`         | Whether plugin is enabled or not                |
| `.PluginReference` | The reference used to push/pull from a registry |

When using the `--format` option, the `plugin ls` command will either
output the data exactly as the template declares or, when using the
`table` directive, includes column headers as well.

The following example uses a template without headers and outputs the
`ID` and `Name` entries separated by a colon (`:`) for all plugins:

```console
$ docker plugin ls --format "{{.ID}}: {{.Name}}"

4be01827a72e: vieux/sshfs:latest
```

To list all plugins in JSON format, use the `json` directive:
```console
$ docker plugin ls --format json
{"Description":"sshFS plugin for Docker","Enabled":false,"ID":"856d89febb1c","Name":"vieux/sshfs:latest","PluginReference":"docker.io/vieux/sshfs:latest"}
```

## Related commands

* [plugin create](plugin_create.md)
* [plugin disable](plugin_disable.md)
* [plugin enable](plugin_enable.md)
* [plugin inspect](plugin_inspect.md)
* [plugin install](plugin_install.md)
* [plugin push](plugin_push.md)
* [plugin rm](plugin_rm.md)
* [plugin set](plugin_set.md)
* [plugin upgrade](plugin_upgrade.md)
