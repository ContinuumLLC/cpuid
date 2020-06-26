# cpuid
Package cpuid provides information about the CPU running the current program.

CPU features are detected on startup, and kept for fast access through the life of the application.
Currently x86 / x64 (AMD64) is supported, and no external C (cgo) code is used, which should make the library very easy to use.

You can access the CPU information by accessing the shared CPU variable of the cpuid library.

Package home: https://github.com/klauspost/cpuid

[![GoDoc][1]][2] [![Build Status][3]][4]

[1]: https://godoc.org/github.com/klauspost/cpuid?status.svg
[2]: https://godoc.org/github.com/klauspost/cpuid
[3]: https://travis-ci.org/klauspost/cpuid.svg
[4]: https://travis-ci.org/klauspost/cpuid

# features
## CPU Instructions
*  **CMOV** (i686 CMOV)
*  **NX** (NX (No-Execute) bit)
*  **AMD3DNOW** (AMD 3DNOW)
*  **AMD3DNOWEXT** (AMD 3DNowExt)
*  **MMX** (standard MMX)
*  **MMXEXT** (SSE integer functions or AMD MMX ext)
*  **SSE** (SSE functions)
*  **SSE2** (P4 SSE functions)
*  **SSE3** (Prescott SSE3 functions)
*  **SSSE3** (Conroe SSSE3 functions)
*  **SSE4** (Penryn SSE4.1 functions)
*  **SSE4A** (AMD Barcelona microarchitecture SSE4a instructions)
*  **SSE42** (Nehalem SSE4.2 functions)
*  **AVX** (AVX functions)
*  **AVX2** (AVX2 functions)
*  **FMA3** (Intel FMA 3)
*  **FMA4** (Bulldozer FMA4 functions)
*  **XOP** (Bulldozer XOP functions)
*  **F16C** (Half-precision floating-point conversion)
*  **BMI1** (Bit Manipulation Instruction Set 1)
*  **BMI2** (Bit Manipulation Instruction Set 2)
*  **TBM** (AMD Trailing Bit Manipulation)
*  **LZCNT** (LZCNT instruction)
*  **POPCNT** (POPCNT instruction)
*  **AESNI** (Advanced Encryption Standard New Instructions)
*  **CLMUL** (Carry-less Multiplication)
*  **HTT** (Hyperthreading (enabled))
*  **HLE** (Hardware Lock Elision)
*  **RTM** (Restricted Transactional Memory)
*  **RDRAND** (RDRAND instruction is available)
*  **RDSEED** (RDSEED instruction is available)
*  **ADX** (Intel ADX (Multi-Precision Add-Carry Instruction Extensions))
*  **SHA** (Intel SHA Extensions)
*  **AVX512F** (AVX-512 Foundation)
*  **AVX512DQ** (AVX-512 Doubleword and Quadword Instructions)
*  **AVX512IFMA** (AVX-512 Integer Fused Multiply-Add Instructions)
*  **AVX512PF** (AVX-512 Prefetch Instructions)
*  **AVX512ER** (AVX-512 Exponential and Reciprocal Instructions)
*  **AVX512CD** (AVX-512 Conflict Detection Instructions)
*  **AVX512BW** (AVX-512 Byte and Word Instructions)
*  **AVX512VL** (AVX-512 Vector Length Extensions)
*  **AVX512VBMI** (AVX-512 Vector Bit Manipulation Instructions)
*  **AVX512VBMI2** (AVX-512 Vector Bit Manipulation Instructions, Version 2)
*  **AVX512VNNI** (AVX-512 Vector Neural Network Instructions)
*  **AVX512VPOPCNTDQ** (AVX-512 Vector Population Count Doubleword and Quadword)
*  **GFNI** (Galois Field New Instructions)
*  **VAES** (Vector AES)
*  **AVX512BITALG** (AVX-512 Bit Algorithms)
*  **VPCLMULQDQ** (Carry-Less Multiplication Quadword)
*  **AVX512BF16** (AVX-512 BFLOAT16 Instructions)
*  **AVX512VP2INTERSECT** (AVX-512 Intersect for D/Q)
*  **MPX** (Intel MPX (Memory Protection Extensions))
*  **ERMS** (Enhanced REP MOVSB/STOSB)
*  **RDTSCP** (RDTSCP Instruction)
*  **CX16** (CMPXCHG16B Instruction)
*  **SGX** (Software Guard Extensions, with activation details)
*  **VMX** (Virtual Machine Extensions)

## Performance
*  **RDTSCP()** Returns current cycle count. Can be used for benchmarking.
*  **SSE2SLOW** (SSE2 is supported, but usually not faster)
*  **SSE3SLOW** (SSE3 is supported, but usually not faster)
*  **ATOM** (Atom processor, some SSSE3 instructions are slower)
*  **Cache line** (Probable size of a cache line).
*  **L1, L2, L3 Cache size** on newer Intel/AMD CPUs.

## Cpu Vendor/VM
* **Intel**
* **AMD**
* **VIA**
* **Transmeta**
* **NSC**
* **KVM**  (Kernel-based Virtual Machine)
* **MSVM** (Microsoft Hyper-V or Windows Virtual PC)
* **VMware**
* **XenHVM**
* **Bhyve**
* **Hygon**

# installing

```go get github.com/klauspost/cpuid```

# example

```Go
package main

import (
	"fmt"
	"github.com/klauspost/cpuid"
)

func main() {
	// Print basic CPU information:
	fmt.Println("Name:", cpuid.CPU.BrandName)
	fmt.Println("PhysicalCores:", cpuid.CPU.PhysicalCores)
	fmt.Println("ThreadsPerCore:", cpuid.CPU.ThreadsPerCore)
	fmt.Println("LogicalCores:", cpuid.CPU.LogicalCores)
	fmt.Println("Family", cpuid.CPU.Family, "Model:", cpuid.CPU.Model)
	fmt.Println("Features:", cpuid.CPU.Features)
	fmt.Println("Cacheline bytes:", cpuid.CPU.CacheLine)
	fmt.Println("L1 Data Cache:", cpuid.CPU.Cache.L1D, "bytes")
	fmt.Println("L1 Instruction Cache:", cpuid.CPU.Cache.L1D, "bytes")
	fmt.Println("L2 Cache:", cpuid.CPU.Cache.L2, "bytes")
	fmt.Println("L3 Cache:", cpuid.CPU.Cache.L3, "bytes")

	// Test if we have a specific feature:
	if cpuid.CPU.SSE() {
		fmt.Println("We have Streaming SIMD Extensions")
	}
}
```

Sample output:
```
>go run main.go
Name: Intel(R) Core(TM) i5-2540M CPU @ 2.60GHz
PhysicalCores: 2
ThreadsPerCore: 2
LogicalCores: 4
Family 6 Model: 42
Features: CMOV,MMX,MMXEXT,SSE,SSE2,SSE3,SSSE3,SSE4.1,SSE4.2,AVX,AESNI,CLMUL
Cacheline bytes: 64
We have Streaming SIMD Extensions
```

# private package

In the "private" folder you can find an autogenerated version of the library you can include in your own packages.

For this purpose all exports are removed, and functions and constants are lowercased.

This is not a recommended way of using the library, but provided for convenience, if it is difficult for you to use external packages.

# license

This code is published under an MIT license. See LICENSE file for more information.
