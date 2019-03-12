# pfastq-dump

[![DOI](https://zenodo.org/badge/92937657.svg)](https://zenodo.org/badge/latestdoi/92937657)

`pfastq-dump` is a bash implementation of [parallel-fastq-dump](https://github.com/rvalieris/parallel-fastq-dump), parallel `fastq-dump` wrapper. `--stdout` option is additionally supported, but almost same features. It also uses `-N` and `-X` options of fastq-dump to specify blocks of data to be decompressed separately.

## Install

Clone this repository and change permission.

```
$ git clone https://github.com/inutano/pfastq-dump
$ cd pfastq-dump
$ chmod a+x bin/pfastq-dump
```

then copy/move to wherever you like.

### Prerequisites

Install [sra-tools](https://github.com/ncbi/sra-tools) and make sure two binaries below are in `$PATH`.

- `fastq-dump`
- `sra-stat`

## Usage

Basic usage:

```
$ pfastq-dump -t <number of threads> [options] <path to .sra file> [<path> ...]
```

It is strongly recommended here as well that the `.sra` data should be downloaded via aspera-connect/ftp/prefetch command, not by fastq-dump.

```
$ pfastq-dump --threads 8 --outdir /path/to/outdir /path/to/SRR000001.sra
```

`--stdout` option is to send decompressed reads to stdout, but may not work in the way you expected. `pfastq-dump` once creates separated files on the disk, then just `cat` them. This feature is to be used in a workflow that requires stdout input of sequence reads, and marge multiple fastq files into one file.

```
$ pfastq-dump --threads 8 --stdout /path/to/**/*sra > data.fastq
```

Mind the disk usage as well as the original implementation warns, this script requires double of decompressed fastq file size.

## Docker container

Available on [Quay.io](https://quay.io/repository/inutano/sra-toolkit)

```
$ ls
mydata.sra
$ docker run --rm -v "$(pwd)":/data -w /data quay.io/inutano/sra-toolkit:v2.9.2 pfastq-dump --threads 8 /data/mydata.sra
```

## Copyright

Copyright (c) 2017 Tazro Inutano Ohta. See LICENSE.txt for further details.
