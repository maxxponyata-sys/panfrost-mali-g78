# panfrost: Mali-G78 on Exynos 2100

Kernel panfrost support for the Arm Mali-G78 (GPU_ID 0x9202) and ACE-Lite cache coherency on Exynos 2100.

`0004` adds the G78 model (Valhall job manager, G57/G68 feature set). `0009` makes panfrost pick ACE-Lite on `samsung,exynos2100-mali` (the GPU also advertises full ACE). Without coherency the GPU reads stale buffers: AFBC garbage and `DATA_INVALID_FAULT` in GTK4. With it: no faults, readback about twice as fast. The device tree must mark the GPU `dma-coherent` (see `exynos2100-o1s-dts`). Userspace needs the Mesa patch in `mesa-panfrost-g78`.

Tested on the Samsung Galaxy S21 (o1s, SM-G991B, Exynos 2100) running mainline Linux 7.4-rc2.

## Patches

Against Linux 7.4-rc2 (`b3dfbde62`):

- `patches/0004-drm-panfrost-add-Mali-G78.patch`
- `patches/0009-drm-panfrost-use-ACE-Lite-coherency-on-Exynos-2100.patch`

```sh
cd linux && git am /path/to/panfrost-mali-g78/patches/*.patch
```

Part of the Galaxy S21 mainline bring-up, index and full build: [linux-o1s](https://github.com/maxxponyata-sys/linux-o1s).

## License

GPL-2.0 (same as the code it patches).
