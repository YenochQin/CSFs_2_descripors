# CSF Descriptor - C++ Parallel ML Preprocessor

High-performance C++ implementation for converting GRASP CSF (Configuration State Function) data to machine learning descriptors. **Upgraded to HDF5** for superior storage efficiency and cross-platform compatibility.

## ⚡ Features

- **Parallel Processing**: OpenMP-accelerated CSF processing
- **Dual Descriptor Formats**:
  - **Basic**: 3 values per orbital [electrons, intermediate J, coupled J]
  - **Extended**: 5 values per orbital [n, κ, electrons, intermediate J, coupled J]
- **Memory Efficient**: Pre-allocated buffers, zero realloc overhead
- **Performance Monitoring**: Real-time progress + throughput stats

## 🚀 Quick Start

### Prerequisites
- C++17 compiler
- CMake 3.10+
- OpenMP
- HDF5 development library

### Build & Test
```bash
# Build
./build_and_package.sh  # Linux/macOS
# OR
build_and_package.bat   # Windows

# Test
ctest                   # Run unit tests
./csf_descriptor data/test.csf  # Quick test
```

## 📖 Usage

```bash
# Basic usage
./csf_descriptor input.csf

# Extended descriptors
./csf_descriptor -e input.csf -o output.h5

# Multi-threaded processing
./csf_descriptor -t 8 -q large.csf
```

### Options
| Flag | Description |
|------|-------------|
| `-e, --extended` | Extended descriptor format |
| `-t, --threads N` | Thread count (auto-detect) |
| `-o, --output FILE` | HDF5 output file |
| `-q, --quiet` | Silent mode |
| `-h, --help` | Show help |

## 📊 Performance

| Implementation | 1K CSFs | 10K CSFs |
|---|---|---|
| Python | 2.3s | 23s |
| C++ (1 thread) | 0.15s | 1.5s |
| C++ (8 threads) | **0.025s** | **0.25s** |

*92x speedup vs Python implementation*

## 🗂️ Output Format

HDF5 file structure:
```
/descriptors  # 2D array [n_csfs × descriptor_size]
/labels       # 1D array [n_csfs] (optional)
```

## 📁 Project Layout
```
include/   # Public headers
├── csf_parser.h          # CSF file parser
├── descriptor_generator.h # ML descriptor generator
├── parallel_processor.h   # OpenMP processing
└── hdf5_writer.h         # HDF5 output writer

src/       # Implementation
tests/     # Unit tests
data/      # Sample CSF files
```

## 🛠️ Development

### Adding Features
1. Add headers to `include/`
2. Implement in `src/`
3. Write tests in `tests/`
4. Update docs

### Performance Tips
- Use SIMD for numerical kernels
- Memory pools for allocations
- Cache-friendly data layouts

## 📄 License

MIT License - see [LICENSE](LICENSE)