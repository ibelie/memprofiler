# MemProfiler

It a non-intrusive tracing and analysis system designed for fragmentation diagnosis. The system captures fine-grained memory events
and reconstructs heap layout snapshots through offline replay. 
MemProfiler implements this decoupled design through two core components: the Online Tracer, operating as a daemon for
runtime event capture, and the Offline Analyzer, responsible for log parsing and state reconstruction. These components interact via a serialized log interface, ensuring high scalability and low coupling.

## Environment

- Ubuntu 22.04
- CMake 3.22.1
- GCC 12.3.0
- Python >= 3.12

### Clone the Repository

```bash
git clone https://github.com/ibelie/memprofiler.git
cd memprofiler
```

### Install Dependencies

```bash
sudo apt update
sudo apt install -y cmake build-essential libdw-dev libelf-dev libzstd-dev libunwind-dev libboost-all-dev python3-pip
```

## Quick Started
Run the complete example with one command:



```bash
# Setup environment
bash ./setup_env.sh
# Run example `test_case`
bash ./run.sh
```

This will automatically:
1. Install all dependencies
2. Build a test case
3. run the tracer
4. Analyze the trace data
5. Generate visualization


Click the Green outlined timestamps to see the corresponding memory layout.

Note: If the memory layout doesn't show up, please check if the cursor is in the "Pan" mode. Only normal cursor can click the timestamps.
![Memory Metrics](docs/metrics.png)
![Memory Layout](docs/memory_layout.png)


## Manual Setup

### Clone the Repository

```bash
git clone https://github.com/ibelie/memprofiler.git
cd memprofiler
```

### Install Dependencies

```bash
sudo apt update
sudo apt install -y cmake build-essential libdw-dev libelf-dev libzstd-dev libunwind-dev libboost-all-dev python3-pip
```
### Tracer
The tracer component captures memory events and their contexts using debugging/tracing mechanisms.

For detailed usage, please refer to [Tracer/README.md](Tracer/README.md)

#### Build Tracer

```bash
cd ./Tracer/
bash ./script/build.sh
```

#### Run Trace

```bash
cd build/
./src/mprofiler --category /name --no-print-save --no-print-stack ./test/test_case 
```
**Note:** 
The target executable must be compiled with debug symbols enabled (e.g., using `-g` flag with GCC) for the tracer to capture stack traces properly.
Output tracedata will be in `Tracer/build/tracedata/test_case/`

### Analyzer
The analyzer processes trace data for offline analysis.

For detailed usage, please refer to [Analyzer/README.md](Analyzer/README.md)

#### Setup Python Environment 

```bash
# Install uv package manager
python -m pip install uv
# Install Python Dependencies
cd ../../Analyzer/
uv sync
```

#### Run Analyzer

```bash
bash script/run_analyzer.sh
```
**Note:** 
the default input path of the trace log is for the test_case, you can change it in the script 

#### Run Visualizer

```bash
uv run visualizer/metrics_plotter.py --base-dir ../Tracer/build/ --benchmark-name test_case
```

## License

This project is licensed under the BSD 2-Clause License - see the [LICENSE](LICENSE) file for details.

## Copyright

Copyright (c) 2026, Chen Jie, Joungtao, Xuzheng Jiang. All rights reserved.
