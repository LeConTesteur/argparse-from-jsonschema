# argparse-from-jsonschema 

Parse Argument with JSON Schema.

## Installation

Need Python 3.6+.

```bash
pip install argparse-from-jsonschema
```

## Usage

[Schema](./tests/argument_config.json):

```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "title": "argument_config",
  "description": "Arg-parse Schema UnitTest",
  "type": "object",
  "properties": {
    "config": {
      "type": "string",
      "positional": true,
      "description": "path of config file"
    },
    "resume": {
      "type": "boolean",
      "description": "resume from checkpoint or not"
    },
    "foo": {
      "type": "boolean",
      "description": "--with-foo for true or --no-foo for false",
      "false-prefix": "no",
      "true-prefix": "with"
    },
    "scale": {
      "type": "number",
      "default": 1.0,
      "description": "scale of image"
    },
    "mode": {
      "enum": [
        "happy",
        "high",
        "heaven"
      ],
      "default": "happy",
      "description": "speed mode"
    },
    "composition": [
      {"type": "string", "description": "Only first element is use"},
      {"type": "null"}
    ]
  },
  "required": [
    "config"
  ]
}
```

Python Code:

```python
# demo.py
import argparse_from_jsonschema

print(argparse_from_jsonschema.parse(schema='./tests/argument_config.json'))
```

Run with arguments:

```bash
python3 demo.py /path/to/config.py
#> {'config': '/path/to/config.py', 'resume': False, 'foo': False 'scale': 1.0, 'mode': 'happy'}
```

CLI:

```bash
argparse-from-jsonschema tests/argument_config.json
#> Show help for schema file [tests/argument_config.json]:
#> usage: YOUR-COMMAND [-h] [--resume] [--with-foo] [--scale SCALE]
#>                     [--mode {happy,high,heaven}]
#>                     config
#>
#> Arg-parse Schema UnitTest
#>
#> positional arguments:
#>   config                path of config file
#>
#> optional arguments:
#>   -h, --help            show this help message and exit
#>   --resume              resume from checkpoint or not
#>   --with-foo, --no-foo  --with-foo for true or --no-foo for false
#>   --scale SCALE         scale of image, [1.0] in default
#>   --mode {happy,high,heaven}
#>                         speed mode, [happy] in default
```
