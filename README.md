# AdvancedDatabases5242
CSE 5242 Project 2 explores high-performance database search/join in C. It benchmarks standard, branchless, 4-way interleaved, and AVX2 SIMD lower-bound binary searches, then applies them to band joins over sorted data. Includes aligned memory setup and microsecond-level timing for throughput comparison.

This project implements and evaluates high-performance search and join techniques for database-style workloads in C, with a focus on reducing branch overhead and improving CPU utilization.

It generates randomized int64_t datasets, sorts the main data array, and benchmarks several binary-search variants: a standard lower-bound binary search, branchless versions using arithmetic and bit masking, a 4-way interleaved scalar version, and an AVX2 SIMD implementation that processes four probes in parallel. The goal is to compare how branch elimination and vectorization affect latency and throughput.

The code also implements a *band join*: for each value p in one relation, it finds all values in the other relation within [p - bound, p + bound]. It does this by using a lower-bound binary search to locate the first candidate and then scanning forward until values leave the range. Two versions are included: a scalar/4-way approach and a SIMD-assisted version using vectorized lower-bound lookups. Both enforce an output-capacity limit (result_size) and return early if the result buffers fill.

A timing harness measures performance of bulk search and join loops in microseconds and reports normalized metrics such as microseconds per search and per outer record. Memory is 64-byte aligned (posix_memalign) for better vectorization behavior, and repeats are supported for more stable measurements.

Overall, the project demonstrates practical DBMS optimization techniques: lower-bound indexing for range queries, branchless control-to-data dependency conversion, instruction-level parallelism via interleaving, and SIMD acceleration with AVX2 intrinsics. It is essentially a systems-performance study of how algorithmic and microarchitectural choices impact search and join speed.
