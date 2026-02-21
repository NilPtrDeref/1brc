# 1BRC - The One Billion Row Challenge (Go Edition)

This repository contains my Go implementation of the [One Billion Row Challenge](https://github.com/gunnarmorling/1brc). The goal is to process a text file containing 1,000,000,000 measurements (station name and temperature) and calculate the min, max, and mean temperature per station as quickly as possible.

## Performance Journey

The challenge is a great way to explore Go's performance characteristics, from standard library usage to low-level optimizations like custom hash maps and manual I/O buffering. 

I have structured my solution into several steps, each living in its own branch. This allows you to follow the progression from a naive implementation to a highly optimized one.

### Optimization Steps

| Branch | Description |
| :--- | :--- |
| [**naive**](https://github.com/NilPtrDeref/1brc/tree/naive) | The baseline single-threaded approach using standard library tools. |
| [**1-buffersize**](https://github.com/NilPtrDeref/1brc/tree/1-buffersize) | Increasing read buffer sizes to improve I/O throughput. |
| [**2-splitalloc**](https://github.com/NilPtrDeref/1brc/tree/2-splitalloc) | Reducing memory allocations and pre-allocating where possible. |
| [**3-readline**](https://github.com/NilPtrDeref/1brc/tree/3-readline) | Moving from `bufio.Scanner` to a more efficient manual reading loop. |
| [**4-fastparsefloat**](https://github.com/NilPtrDeref/1brc/tree/4-fastparsefloat) | Implementing a custom float parser to avoid `strconv.ParseFloat` overhead. |
| [**5-fastsearchsemi**](https://github.com/NilPtrDeref/1brc/tree/5-fastsearchsemi) | Optimizing the search for the semicolon separator. |
| [**6-fasterparsefloat**](https://github.com/NilPtrDeref/1brc/tree/6-fasterparsefloat) | Further refinements to the temperature parsing logic. |
| [**7-manualbuffering**](https://github.com/NilPtrDeref/1brc/tree/7-manualbuffering) | Bypassing `bufio` for manual buffer management and zero-copy operations. |
| [**8-fastsearchnl**](https://github.com/NilPtrDeref/1brc/tree/8-fastsearchnl) | Optimizing newline detection to speed up row scanning. |
| [**9-customhashmap**](https://github.com/NilPtrDeref/1brc/tree/9-customhashmap) | Replacing `map[string]` with a specialized, performance-oriented hash map. |
| [**master**](https://github.com/NilPtrDeref/1brc/tree/master) | The current "best" version incorporating all successful optimizations. |

---

## Getting Started

### Prerequisites

- [Go](https://go.dev/doc/install) 1.21 or higher.

### 1. Generate the Data

First, you need to generate the `measurements.txt` file. You can generate any number of rows, but the challenge standard is one billion.

```bash
# Generate 1 billion rows (Warning: This will create a ~13GB file)
go run main.go generate -c 1000000000 -o measurements.txt
```

### 2. Run the Challenge

To run the processing and measure the execution time:

```bash
time go run main.go
```

## Running Tests

Basic validation can be run using:

```bash
go test ./...
```

## References

- [Original 1BRC Repository (Java)](https://github.com/gunnarmorling/1brc)
- [The Go Performance Community](https://github.com/benhoyt/1brc) - Great resource for Go-specific 1BRC optimizations.
