# Performance and scalability
- **Expected load**: 200 decisions/day, peak 10 concurrent.
- **Resource sizing**: 8 vCPU / 32 GB RAM per solver run.
- **Concurrency limits**: 10 parallel runs per cluster.
- **Backpressure behavior**: queue with FIFO; reject if queue depth > 100.
- **Capacity assumptions**: deterministic single-precision is not allowed; float64 strict.
