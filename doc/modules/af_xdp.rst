AF_XDP Support
==============

PF_RING since version 7.5 includes support for AF_XDP adapters,
however this is available from source code only, using the --enabled-xdp
configure flag when compiling the userspace library.

Prerequisite
------------

- Dependencies: libelf-dev
- Kernel: >4.18 (5.1.2 is tested and recommended), configured with `CONFIG_XDP_SOCKETS=y`
- libbpf with latest AF_XDP support installed from <kernel source>/tools/lib/bpf

Installation
------------

Install a kernel >4.18.

Ubuntu 20 already runs a kernel 5.x which is supported by AF_XDP. On Ubuntu 18.04 you can
use uktools available at https://github.com/usbkey9/uktools/ following the instructions below:

.. code-block:: console

   git clone https://github.com/usbkey9/uktools && cd uktools
   make

Download and unpack sources for kernel 5.x, it will be used to compile and install libbpf:

.. code-block:: console

   wget http://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.1.2.tar.xz
   tar xvf linux-5.1.2.tar.xz 
   cd linux-5.1.2/tools/lib/bpf
   sudo apt-get install libelf-dev
   make
   sudo make install_lib
   sudo make install_headers

Compile PF_RING with AF_XDP support (note: --enable-xdp was required on previous pf_ring versions):

.. code-block:: console

   cd PF_RING/userland
   ./configure --enable-xdp
   make

Usage
-----

You are now ready to run any pf_ring sample application using xdp:<interface>@<queue> as interface name.
Example:

.. code-block:: console

   cd examples
   sudo ./pfcount -i xdp:eth1@0

Please note that with AF_XDP pf_ring attaches to a NIC queue, please disable RSS or open all queues.
