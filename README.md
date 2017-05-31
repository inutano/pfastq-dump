# pfastq-dump

`pfastq-dump` is a bash implementation of [parallel-fastq-dump](https://github.com/rvalieris/parallel-fastq-dump), parallel `fastq-dump` wrapper. `--stdout` option is additionally supported, but almost same features. It also uses `-N` and `-X` options of fastq-dump to specify blocks of data to be decompressed separately.

## Install

Clone this repository and change permission.

```
$ git clone https://github.com/inutano/pfastq-dump
$ cd pfastq-dump
$ chmod a+x bin/pfastq-dump
```

then copy/move to wherever you like.

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
