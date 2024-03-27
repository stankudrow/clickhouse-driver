# ClickHouse Python Driver

[![PyPI version](https://img.shields.io/pypi/v/clickhouse-driver.svg "PyPI version")](https://pypi.org/project/clickhouse-driver)
[![Coverage](https://coveralls.io/repos/github/mymarilyn/clickhouse-driver/badge.svg?branch=master "Coverage")](https://coveralls.io/github/mymarilyn/clickhouse-driver?branch=master)
[![License](https://img.shields.io/pypi/l/clickhouse-driver.svg "License")](https://pypi.org/project/clickhouse-driver)
[![Python versions](https://img.shields.io/pypi/pyversions/clickhouse-driver.svg "Python versions")](https://pypi.org/project/clickhouse-driver)
[![Downloads](https://img.shields.io/pypi/dm/clickhouse-driver.svg "Downloads")](https://pypi.org/project/clickhouse-driver)
[![Build status](https://github.com/mymarilyn/clickhouse-driver/actions/workflows/actions.yml/badge.svg "Build status")](https://github.com/mymarilyn/clickhouse-driver/actions/workflows/actions.yml)

The ClickHouse Python Driver with the native (TCP) interface support.

The asynchronous wrapper for this project is available [here](https://github.com/mymarilyn/aioch).

## Features

- External data for query processing.

- Query settings.

- Compression support.

- TLS support.

- Supported types:

  - Float32/64
  - [U]Int8/16/32/64/128/256
  - Date/Date32/DateTime('timezone')/DateTime64('timezone')
  - String/FixedString(N)
  - Enum8/16
  - Array(T)
  - Nullable(T)
  - Bool
  - UUID
  - Decimal
  - IPv4/IPv6
  - LowCardinality(T)
  - SimpleAggregateFunction(F, T)
  - Tuple(T1, T2, ...)
  - Nested
  - Map(key, value)

- Query progress information.

- Block by block results streaming.

- Reading query profile info.

- Receiving server logs.

- Multiple hosts support.

- Python DB API 2.0 specification support.

- Optional NumPy arrays support.

## Documentation

The documentation is available [here](https://clickhouse-driver.readthedocs.io).

## Usage

There are two ways to communicate with a server using:

- a pure Client;
- the DB API.

The pure Client example:

```python
>>> from clickhouse_driver import Client
>>>
>>> client = Client('localhost')
>>>
>>> client.execute('SHOW TABLES')
[('test',)]
>>> client.execute('DROP TABLE IF EXISTS test')
[]
>>> client.execute('CREATE TABLE test (x Int32) ENGINE = Memory')
[]
>>> client.execute(
...     'INSERT INTO test (x) VALUES',
...     [{'x': 100}]
... )
1
>>> client.execute('INSERT INTO test (x) VALUES', [[200]])
1
>>> client.execute(
...     'INSERT INTO test (x) '
...     'SELECT * FROM system.numbers LIMIT %(limit)s',
...     {'limit': 3}
... )
[]
>>> client.execute('SELECT sum(x) FROM test')
[(303,)]
```

The DB API example:

```python
>>> from clickhouse_driver import connect
>>>
>>> conn = connect('clickhouse://localhost')
>>> cursor = conn.cursor()
>>>
>>> cursor.execute('SHOW TABLES')
>>> cursor.fetchall()
[('test',)]
>>> cursor.execute('DROP TABLE IF EXISTS test')
>>> cursor.fetchall()
[]
>>> cursor.execute('CREATE TABLE test (x Int32) ENGINE = Memory')
>>> cursor.fetchall()
[]
>>> cursor.executemany(
...     'INSERT INTO test (x) VALUES',
...     [{'x': 100}]
... )
>>> cursor.rowcount
1
>>> cursor.executemany('INSERT INTO test (x) VALUES', [[200]])
>>> cursor.rowcount
1
>>> cursor.execute(
...     'INSERT INTO test (x) '
...     'SELECT * FROM system.numbers LIMIT %(limit)s',
...     {'limit': 3}
... )
>>> cursor.rowcount
0
>>> cursor.execute('SELECT sum(x) FROM test')
>>> cursor.fetchall()
[(303,)]
```

## License

The ClickHouse Python Driver is distributed under the [MIT](http://www.opensource.org/licenses/mit-license.php) license.
