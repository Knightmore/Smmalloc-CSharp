<p align="center"> 
  <img src="https://i.imgur.com/7XvtEWf.png" alt="alt logo">
</p>

[![PayPal](https://drive.google.com/uc?id=1OQrtNBVJehNVxgPf6T6yX1wIysz1ElLR)](https://www.paypal.me/nxrighthere) [![Bountysource](https://drive.google.com/uc?id=19QRobscL8Ir2RL489IbVjcw3fULfWS_Q)](https://salt.bountysource.com/checkout/amount?team=nxrighthere)

This is an improved version of [smmalloc](https://github.com/SergeyMakeev/smmalloc) a [fast and efficient](https://github.com/SergeyMakeev/smmalloc#features) memory allocator designed to handle many small allocations/deallocations in heavy multi-threaded scenarios. The allocator created for using in applications where the performance is critical such as video games.

Using smmalloc allocator in the .NET environment helps to minimize GC pressure for allocating buffers and avoid using lock-based pools in multi-threaded systems. Modern .NET features such as `Span<T>` greatly works in tandem with smmalloc and allows conveniently manage data in native memory.

Usage
--------
##### Create a new smmalloc instance:
```c#
// 8 buckets, 16 MB each, 128 bytes maximum allocation size
SmmallocInstance smmalloc = new SmmallocInstance(8, 16 * 1024 * 1024);
```

##### Create thread cache for a current thread:
```c#
// 4 KB of thread cache for each bucket, hot warmup
smmalloc.CreateThreadCache(CacheWarmupOptions.Hot, 4 * 1024);
```

##### Allocate aligned memory block:
```c#
// 64 bytes of a memory block
IntPtr pointer = smmalloc.Malloc(64);
```

##### Write data to memory block:
```c#
// Using Marshal
byte data = 0;

for (int i = 0; i < 64; i++) {
	Marshal.WriteByte(pointer, i, data++);
}

// Using Span
Span<byte> buffer;

unsafe {
	buffer = new Span<byte>((byte*)pointer, 64);
}

byte data = 0;

for (int i = 0; i < buffer.Length; i++) {
	buffer[i] = data++;
}
```

##### Read data from memory block:
```c#

```

API reference
--------
