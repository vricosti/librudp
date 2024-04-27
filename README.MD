# librudp


Preferred method for now (until cmake):  

```bash
meson setup builddir -Dtests=true
ninja -C builddir
```

You can still traditional autotools if you really know it:  

```bash
autoreconf -vi && ./configure
make
```