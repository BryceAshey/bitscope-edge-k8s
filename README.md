# bitscope-edge-k8s

In this tutorial I will be setting up BitScope's 24-node Edge Rack and 24 Raspberry PIs for running Kubernetes (k8s) to replace my previous k8s cluster. You can see that original build under the [raspberry-pi-kubernetes-cluster](https://github.com/BryceAshey/raspberry-pi-kubernetes-cluster) repository on my [GitHub](https://github.com/BryceAshey).

## Tools

I'm running Windows 10 for my desktop so I'm going to use the following tools. If you're on a Mac you can install most of these using Brew.

- [Docker Desktop](https://www.docker.com/products/docker-desktop)
  - [For Windows: Ubuntu WSL](https://ubuntu.com/wsl)
  - Be sure to setup a hub.docker.com account.
- [GitHub](https://desktop.github.com)
- [Kubernetes on Windows](https://kubernetes.io/blog/2020/05/21/wsl-docker-kubernetes-on-the-windows-desktop/)
- [Lens Kubernetes IDE](https://k8slens.dev/)
- [Microsoft's VSCode](https://code.visualstudio.com/download)
- [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701#activetab=pivot:overviewtab)

## Bill of Materials (BOM)

- [BitScope ER24A Edge Rack](https://www.bitscope.com)* (x1) ($1685)
- [XP Power VES300PS24 (300W/24V Power Supply)](https://www.digikey.com/en/products/detail/xp-power/VES300PS24/11494506?s=N4IgTCBcDaIG4FMDOBmADGgDksAWEAugL5A) (x2) ($184.00)
- [Raspberry PI 4 8GB](https://vilros.com/products/raspberry-pi-4-model-b-8gb-ram) (x24) ($1,800)
- [3' Ethernet Cable](https://www.amazon.com/dp/B01I06BXQU?ref_=pe_386300_442746000_DDT_E_DDE_dt_1) (x2 packs of 20) ($79.72)
- [NetGear 26-port Gigabit Ehternet Switch](https://www.amazon.com/gp/product/B07P8BHZM9/ref=ox_sc_act_title_2?smid=ATVPDKIKX0DER&psc=1) (x1) ($157.10)
  - [AGM734 SFP (for uplink)](https://www.amazon.com/gp/product/B01FRQJ1Y2/ref=ewc_pr_img_1?smid=AE2OZG2NN3099&psc=1) (x1) ($21.99)
- [Tripp Lite 13U 2-Post Open Frame Rack](https://www.amazon.com/gp/product/B00NOTXZZG/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1) (x1) ($130.77)
- [Synology DS920+ Bundle](https://www.amazon.com/Synology-DiskStation-DS920-Diskless-4-bay/dp/B087Z34F3R) (x1) ($744.97)
  - [Synology 4GB Ram Module](https://www.amazon.com/Synology-RAM-DDR4-2666-Non-ECC-SO-DIMM/dp/B07X5MMGWG/ref=pd_bxgy_2/133-7492947-2350765?pd_rd_w=Jqagu&pf_rd_p=c64372fa-c41c-422e-990d-9e034f73989b&pf_rd_r=AQ8X4062VFD2X8RZ38QT&pd_rd_r=fba2d0cf-9cab-407a-8879-88a89b0c2c44&pd_rd_wg=avwhZ&pd_rd_i=B07X5MMGWG&psc=1) (x1)
  - [Seagate 4TB NAS Internal HDD](https://www.amazon.com/Seagate-IronWolf-5900RPM-Internal-3-5-Inch/dp/B07H289S79/ref=pd_bxgy_1/133-7492947-2350765?pd_rd_w=Jqagu&pf_rd_p=c64372fa-c41c-422e-990d-9e034f73989b&pf_rd_r=AQ8X4062VFD2X8RZ38QT&pd_rd_r=fba2d0cf-9cab-407a-8879-88a89b0c2c44&pd_rd_wg=avwhZ&pd_rd_i=B07H289S79&psc=1) (x4)

**Notes:**

- Total cost (list prices, no shipping or tax): $4,803.55
- *A note on the Edge Racks - they do come in 12 node sizes if that fits closer to your budget
- **I've had a Synology DS918+ for a while and now and will be using that. However, the current version is the DS920+ so quoting the DS920+ here.

## Overview

This build provides for 96 cores and 192GB of RAM however we will utilize three of the nodes for cluster masters leaving us with 84 cores and 168GB of RAM for the workers.