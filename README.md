# Singlejson parser plugin for Embulk

TODO: Write short description here and embulk-parser-singlejson.gemspec file.

## Overview

* **Plugin type**: parser
* **Guess supported**: no

## Configuration

- **shema**: description (array, required)

## Example

config.yml
```yaml
in:
  type: file
  path_prefix: sample.json
  parser:
    type: singlejson
    schema:
      - {name: hello, type: string, exp: "json['hello']"}
      - {name: num, type: long, exp: "json['one']"}
      - {name: add, type: string, exp: "json['hello'] + json['one'].to_s"}
      - {name: dir, type: string, exp: "json['parent']['child']['grandchild']"}
exec: {}
out:
  type: stdout
```

sample.json
```json
{
    "hello": "world",
    "one": 1,
    "parent":{
        "child": {
            "grandchild": "ok"
        }
    }
}
```

result
```
+--------------+----------+------------+------------+
| hello:string | num:long | add:string | dir:string |
+--------------+----------+------------+------------+
|        world |        1 |     world1 |         ok |
+--------------+----------+------------+------------+
```

(If guess supported) you don't have to write `parser:` section in the configuration file. After writing `in:` section, you can let embulk guess `parser:` section using this command:

```
$ embulk gem install embulk-parser-singlejson
$ embulk guess -g singlejson config.yml -o guessed.yml
```

## Build

```
$ rake
```
