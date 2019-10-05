An extension of cpython's [wave.py](https://github.com/python/cpython/blob/master/Lib/wave.py) submodule supporting CCITT G.711 ulaw, alaw. Essentially a port of Eric Woudenberg's [wave.py patch submission for python 2](https://bugs.python.org/issue1483545)[file](https://bugs.python.org/file36465/wave.py)), except here the original 'NONE' compression type is retained (is not renamed 'PCM' as in Eric's patch).

Diff:
```
81a82,83
> WAVE_FORMAT_ALAW  = 0x0006
> WAVE_FORMAT_MULAW = 0x0007
261,267c263,270
<             try:
<                 sampwidth = struct.unpack_from('<H', chunk.read(2))[0]
<             except struct.error:
<                 raise EOFError from None
<             self._sampwidth = (sampwidth + 7) // 8
<             if not self._sampwidth:
<                 raise Error('bad sample width')
---
>             self._comptype = 'NONE'
>             self._compname = 'not compressed'
>         elif wFormatTag == WAVE_FORMAT_MULAW:
>             self._comptype = 'ULAW'
>             self._compname = 'CCITT G.711 u-law'
>         elif wFormatTag == WAVE_FORMAT_ALAW:
>             self._comptype = 'ALAW'
>             self._compname = 'CCITT G.711 a-law'
269a273,281
>         try:
>             sampwidth = struct.unpack_from('<H', chunk.read(2))[0]
>         except struct.error:
>             raise EOFError from None
>         self._sampwidth = (sampwidth + 7) // 8
>         if not self._sampwidth:
>             raise Error('bad sample width')
>         if self._comptype in ['ULAW', 'ALAW'] and self._sampwidth != 1:
>             raise Error(f'invalid {self._comptype} sample width: found {self._sampwidth}, should be 1')
273,274d284
<         self._comptype = 'NONE'
<         self._compname = 'not compressed'
```
