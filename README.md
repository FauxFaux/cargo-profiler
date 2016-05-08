# cargo-profiler
cargo subcommand to profile binaries

## To install

First install valgrind:

```
$ sudo apt-get install valgrind
```

Then, build the binary.
```
$ cargo build --release
```

Now, copy the built binary to the same directory as cargo.

```
$ sudo cp ./target/release/cargo-profiler $(dirname $(which cargo))/
```

## To run

Cargo profiler currently supports callgrind and cachegrind.

```
$ cargo profiler callgrind --bin $PATH_TO_BINARY
$ cargo profiler cachegrind --bin $PATH_TO_BINARY
```

You can limit the number of functions you'd like to look at:

```
$ cargo profiler callgrind --bin ./target/debug/rsmat -n 10

Profiling rsmat with callgrind...

Total Instructions...190,372,956

78,346,775 (41.2%) dgemm_kernel.rs:matrixmultiply::gemm::masked_kernel::h037533f74146b5d9
-----------------------------------------------------------------------
23,528,320 (12.4%) iter.rs:_$LT$std..ops..Range$LT$A$GT$$u20$as$u20$std..iter..Iterator$GT$::next::h3637c345b34d1bb2
-----------------------------------------------------------------------
16,824,925 (8.8%) loopmacros.rs:matrixmultiply::gemm::masked_kernel::h037533f74146b5d9
-----------------------------------------------------------------------
10,236,864 (5.4%) mem.rs:core::mem::swap::h8d5cd2518659f0bb
-----------------------------------------------------------------------
7,712,846 (4.1%) memset.S:memset
-----------------------------------------------------------------------
6,979,680 (3.7%) ops.rs:_$LT$usize$u20$as$u20$ops..Add$GT$::add::hc57472a1060d1f70
-----------------------------------------------------------------------
6,973,791 (3.7%) ptr.rs:core::ptr::_$LT$impl$u20$$BP$const$u20$T$GT$::offset::h686ec191b59fbed7
-----------------------------------------------------------------------
6,049,056 (3.2%) ops.rs:_$LT$$RF$$u27$b$u20$usize$u20$as$u20$ops..Add$LT$$RF$$u27$a$u20$usize$GT$$GT$::add::h09358de14d3353c1
-----------------------------------------------------------------------
3,942,400 (2.1%) wrapping.rs:_$LT$XorShiftRng$u20$as$u20$Rng$GT$::next_u32::h24beecf939a84404
-----------------------------------------------------------------------
3,174,400 (1.7%) mod.rs:core::num::_$LT$impl$u20$u32$GT$::overflowing_shr::haa481f134e0f4bd8
-----------------------------------------------------------------------

```

With cachegrind, you can also sort the data by a particular metric column:

```
$ cargo profiler cachegrind --bin ./target/debug/rsmat -n 10 --sort dr

Profiling rsmat with cachegrind...

Total Instructions...122,760,788

Total I1 Read Misses...37,949,289	Total L1 Read Misses...26,297,409
Total D1 Reads...19,077,797	Total D1 Read Misses...9,255,802
Total DL1 Read Misses...11,411,662	Total Writes...10,702,181
Total D1 Write Misses...7,475,207	Total DL1 Write Misses...4,915,204


 Ir  I1mr ILmr  Dr  D1mr DLmr  Dw  D1mw DLmw
0.64 0.61 0.64 0.54 0.83 0.61 0.57 0.53 0.65 dgemm_kernel.rs:matrixmultiply::gemm::masked_kernel::h037533f74146b5d9
-----------------------------------------------------------------------
0.19 0.17 0.31 0.24 0.06 0.22 0.26 0.16 0.19 mem.rs:core::mem::swap::h8d5cd2518659f0bb
-----------------------------------------------------------------------
0.17 0.22 0.05 0.22 0.11 0.17 0.17 0.32 0.17 ops.rs:_$LT$$RF$$u27$b$u20$usize$u20$as$u20$ops..Add$LT$$RF$$u27$a$u20$usize$GT$$GT$::add::h09358de14d3353c1
-----------------------------------------------------------------------
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 iter.rs:_$LT$std..ops..Range$LT$A$GT$$u20$as$u20$std..iter..Iterator$GT$::next::h3637c345b34d1bb2
-----------------------------------------------------------------------
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 loopmacros.rs:matrixmultiply::gemm::masked_kernel::h037533f74146b5d9
-----------------------------------------------------------------------
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 memset.S:memset
-----------------------------------------------------------------------
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 ptr.rs:core::ptr::_$LT$impl$u20$$BP$const$u20$T$GT$::offset::h686ec191b59fbed7
-----------------------------------------------------------------------
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 wrapping.rs:_$LT$XorShiftRng$u20$as$u20$Rng$GT$::next_u32::h24beecf939a84404
-----------------------------------------------------------------------
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 mod.rs:core::num::_$LT$impl$u20$u32$GT$::overflowing_shr::haa481f134e0f4bd8
-----------------------------------------------------------------------

```



## TODO

* cmp subcommand - compare binary profiles
