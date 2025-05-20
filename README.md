# Byte Chomp

Byte Chomp is a teaching-oriented cache simulator built in Python. It was originally developed for a Computer Architecture course to help students experiment with different cache organizations and see how replacement strategies influence performance.

The simulator reads a binary file containing big-endian 32‑bit memory addresses. As each address is processed the tool tracks hits, compulsory misses (first time a line is loaded), capacity misses (cache too small) and conflict misses (associativity effects). Statistics can be printed either as a compact line of space separated numbers or as a JSON style dictionary.

![Byte Chomp](images/byte_chomp.png)

## Features

* Adjustable number of sets, block size and associativity
* Pluggable replacement strategies:
  * **Random** (``R``)
  * **FIFO** – First In First Out (``F``)
  * **LRU** – Least Recently Used (``L``)
* Reports hit rate, overall miss rate and the breakdown of miss types
* Command line interface with argument validation
* Unit tests covering all replacement policies

## Installation

Python 3.9 or newer is required. Install dependencies with:

```bash
pip install -r requirements.txt
```

Running the tests also requires ``pytest`` which is included in ``requirements.txt``.

## Command Line Usage

```
python cache_simulator.py <n_sets> <b_size> <assoc> <pol> <output_flag> <filename>
```

Where:

| Argument | Description |
|----------|-------------|
| ``n_sets`` | Number of sets in the cache |
| ``b_size`` | Block size in bytes |
| ``assoc`` | Cache associativity |
| ``pol`` | Replacement policy: ``R`` (Random), ``F`` (FIFO) or ``L`` (LRU) |
| ``output_flag`` | ``1`` for compact output, ``0`` for verbose report |
| ``filename`` | Path to a binary file with 32‑bit addresses |

### Example

```
python cache_simulator.py 128 2 4 R 0 tests/addresses/bin_1000.bin
```

The simulator prints a dictionary with the computed statistics.

### Replacement Policies

Replacement algorithms live in ``byte_chomp/replacement_policies.py`` and share a common ``ReplacementStrategy`` base class. Adding a new policy only requires implementing ``select_replacement_slot`` and, optionally, ``update_access``.

## Docker Support

A minimal Dockerfile is included to demonstrate containerisation, but the default command still references ``chomp_bytes.py``. If you plan to build an image adjust the ``CMD`` line to run ``cache_simulator.py`` with your desired parameters.

## Project Layout

```
byte-chomp/
├── byte_chomp/              # Library code
│   ├── argparser.py         # CLI argument parser
│   ├── cache.py             # Cache implementation
│   ├── models.py            # Data models and statistics
│   └── replacement_policies.py  # Replacement strategies
├── cache_simulator.py       # Entry point script
├── tests/                   # Pytest suite and sample address files
├── images/                  # Project logo
├── requirements.txt         # Python dependencies
└── Dockerfile               # Sample container setup
```

## Running the Test Suite

Execute all unit tests with:

```bash
pytest
```

The provided address files allow you to validate the simulator behaviour under different cache configurations.

## License

Byte Chomp is distributed under the GNU General Public License v2. See the [LICENSE](LICENSE) file for full details.

