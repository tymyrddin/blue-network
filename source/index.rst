.. Network mitigations documentation master file, created by
   sphinx-quickstart on Sun Jun 26 05:19:13 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Network mitigations
===============================================

The policies and practices adopted to prevent, monitor, and recover from network attacks. On a general level mostly (for now), and a sprinkle of concreteness based on our own home network.

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Basics

   docs/basics/README.md
   docs/basics/*

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: OpenWRT

   docs/openwrt/README.md
   docs/openwrt/flash-router.md
   docs/openwrt/*

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Wireless

   docs/wireless/README.md
   docs/wireless/signal-strength.md
   docs/wireless/defaults.md
   docs/wireless/broadcasting.md
   docs/wireless/encryption.md
   docs/wireless/wps.md
   docs/wireless/unauthorised.md
   docs/wireless/restrict-by-mac.md
   docs/wireless/certificates.md

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Intranet

   docs/intranet/README.md
   docs/intranet/add-nat.md
   docs/intranet/segmentation.md
   docs/intranet/*

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Internet

   docs/internet/README.md
   docs/internet/*

.. toctree::
   :caption: All mitigations

   Overview <https://tymyrddin.github.io/mitigations/>
