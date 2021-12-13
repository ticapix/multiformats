# `multiformats`: A Python implementation of [multiformat protocols](https://multiformats.io/)

[![Generic badge](https://img.shields.io/badge/python-3.7+-green.svg)](https://docs.python.org/3.7/)
![PyPI version shields.io](https://img.shields.io/pypi/v/multiformats.svg)
[![PyPI status](https://img.shields.io/pypi/status/multiformats.svg)](https://pypi.python.org/pypi/multiformats/)
[![Checked with Mypy](http://www.mypy-lang.org/static/mypy_badge.svg)](https://github.com/python/mypy)
[![Python package](https://github.com/hashberg-io/multiformats/actions/workflows/python-pytest.yml/badge.svg)](https://github.com/hashberg-io/multiformats/actions/workflows/python-pytest.yml)
[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)


This is a fully compliant Python implementation of the [multiformat protocols](https://multiformats.io/).


## Table of Contents

- [Install](#install)
- [Usage](#usage)
- [API](#api)
- [Contributing](#contributing)
- [License](#license)


## Install

You can install the latest release from PyPI as follows:

```
pip install --upgrade multiformats
```

## Usage

### Varint

The `varint` module implements the [unsigned-varint spec](https://github.com/multiformats/unsigned-varint).
Functionality is provided by the `encode` and `decode` functions, converting between non-negative `int` values and the corresponding varint `bytes`: 

```py
>>> from multiformats import varint
>>> varint.encode(128)
b'\x80\x01'
>>> varint.decode(b'\x80\x01')
128
```

For advanced usage, see the [API documentation](https://hashberg-io.github.io/multiformats/multiformats/varint.html). 


### Multicodec

The `multicodec` module implements the [multicodec spec](https://github.com/multiformats/multicodec).
The `Multicodec` dataclass provides a container for multicodec data:

```py
>>> Multicodec.from_json({
...     'name': 'cidv1', 'tag': 'ipld', 'code': '0x01',
...     'status': 'permanent', 'description': 'CIDv1'})
Multicodec(name='cidv1', tag='ipld', code=1,
           status='permanent', description='CIDv1')
```

The `exists` and `get` functions can be used to check whether a multicodec with given name or code is known, and if so to get the corresponding object:

```py
>>> from multiformats import multicodec
>>> multicodec.exists("identity")
True
>>> multicodec.exists(0x01)
True
>>> multicodec.get("identity")
Multicodec(name='identity', tag='multihash', code=0,
           status='permanent', description='raw binary')
>>> multicodec.get(0x01)
Multicodec(name='cidv1', tag='ipld', code=1,
           status='permanent', description='CIDv1')
```

The `table` function can be used to iterate through known multicodecs, optionally restrictiong to one or more tags and/or statuses:

```py
>>> len(list(multicodec.table()))
482
>>> selected = multicodec.table(tag=["ipld", "multiaddr"], status="permanent")
>>> [m.code for m in selected]
[1, 4, 6, 41, 53, 54, 55, 56, 85, 112, 113, 114, 120,
 144, 145, 146, 147, 148, 149, 150, 151, 152, 176, 177,
 178, 192, 193, 290, 297, 400, 421, 460, 477, 478, 479]
```

For advanced usage, see the [API documentation](https://hashberg-io.github.io/multiformats/multiformats/multicodec.html).


## API

The [API documentation](https://hashberg-io.github.io/multiformats/multiformats/index.html) for this package is automatically generated by [pdoc](https://pdoc3.github.io/pdoc/).


## Contributing

Please see [the contributing file](./CONTRIBUTING.md).


## License

[MIT © Hashberg Ltd.](LICENSE)
