# Mesa 3.1 for AMIX

An independent Mesa 3.1 software-rendering port for Commodore Amiga UNIX
(AMIX), using X11R6.3 and the MNT ZZ9000 framebuffer X server.

The repository contains only the AMIX port overlay and deterministic source
installer. Unmodified Mesa sources come from the official Mesa archive.

## Layout

- Git checkout: `C:\Storage\mesa-amix`
- Prepared local tree: `C:\Storage\usr-amix\usr\mesa3\Mesa-3.1`
- AMIX build tree: `/usr/mesa3/Mesa-3.1`
- X11 dependency: `/usr/x11r6`

Mesa is intentionally independent from the X11R6 source tree. The initial
target uses Mesa's Xlib software driver, so it does not require a server-side
GLX extension or ZZ9000 3D acceleration.

## Prepare sources

On a host with HTTPS access, `gzip`, `tar`, and a SHA-256 utility:

```sh
git clone https://github.com/isoriano1968/mesa-amix.git
cd mesa-amix
sh install.sh /export/amix/mesa3
```

Copy or mount `/export/amix/mesa3/Mesa-3.1` as
`/usr/mesa3/Mesa-3.1` on AMIX.

## Initial AMIX build

The upstream Mesa 3.1 distribution already contains an historical AMIX
target. This overlay keeps that target but selects GCC 2.7.2.3 and the
independent X11R6 tree. MIT-SHM, pthreads, CPU assembly, and hardware drivers
are not enabled.

```sh
cd /usr/mesa3/Mesa-3.1
sh amix-build.sh libs
sh amix-build.sh xdemos
sh amix-build.sh mech
```

The first milestone builds static Mesa, GLU, and GLUT libraries to validate
the compiler and Xlib renderer. Native AMIX shared libraries will follow only
after the software renderer and demos work correctly.

For runtime tests against the ZZ9000 server:

```sh
DISPLAY=:1
LD_LIBRARY_PATH=/usr/x11r6/xc/exports/lib:/usr/lib
export DISPLAY LD_LIBRARY_PATH
```

The `mech` target builds the classic GLUT robot demo imported from GLUT 3.7:

```sh
cd /usr/mesa3/Mesa-3.1
sh amix-build.sh mech
./demos/glutmech
```

## Source archives

- `MesaLib-3.1.tar.gz`
- `MesaDemos-3.1.tar.gz`
- `glut-3.7/progs/demos/glutmech/glutmech.c`

The Mesa archives are downloaded from the
[official Mesa archive](https://archive.mesa3d.org/older-versions/3.x/).
The `glutmech` demo comes from the official GLUT 3.7 archive; its original
source header and GLUT notice are kept in the overlay.
