---
title: LXC
tags:
  - containerization
  - virtualization
  - linux
  - system-container
date: 2025-11-16
---

# LXC (Linux Containers)

LXC (Linux Containers) is an operating-system-level virtualization method for running multiple isolated Linux systems (containers) on a control host using a single Linux kernel. It leverages core kernel features like **namespaces** and **cgroups** to provide isolation and resource management, offering a lightweight yet powerful alternative to full hardware virtualization.

Unlike [[docker|Docker]], which focuses on "application containers" designed to run a single process, LXC specializes in **"system containers."** A system container provides an environment that looks and feels like a complete, lightweight virtual machine, with its own `init` process, user space, and the ability to run multiple services.

---

## System Containers vs. Application Containers

The core philosophy of LXC is to provide **system containers**, which contrasts with the **application container** model popularized by [[docker|Docker]]. This distinction is fundamental to understanding its use cases.

For a detailed comparison, see the main explanation in the [[containerization#System Containers vs. Application Containers|Containerization section overview]].

---

## How LXC Works: Core Kernel Features

LXC achieves isolation by using features built directly into the Linux kernel, without the need for a hypervisor.

1.  **Namespaces**: This kernel feature partitions system resources such that one set of processes sees one set of resources, while another set of processes sees a different set. **Crucially, namespaces achieve this isolation by partitioning the host kernel's resources directly, rather than emulating hardware like virtual machines do.** LXC uses several types of namespaces:
    *   **PID Namespace**: Provides isolation for process IDs. Processes inside a container have their own PID 1 (the `init` process) and cannot see processes on the host or in other containers.
    *   **Network Namespace**: Gives each container its own virtual network stack (IP addresses, routing tables, firewall rules).
    *   **Mount Namespace**: Isolates the filesystem mount points. Each container has its own root filesystem.
    *   **User Namespace**: Maps user and group IDs inside the container to different IDs on the host, enhancing security.

2.  **Control Groups (cgroups)**: Think of cgroups as a "resource budget" manager for containers. This kernel feature is responsible for two key things:
    *   **Limiting Resources**: It enforces limits on how many resources a container can use. For example, you can configure a container to use no more than 2 CPU cores and 1GB of RAM. This prevents a single container from monopolizing the host's resources and starving other containers.
    *   **Accounting for Resources**: It keeps track of how many resources a container is actually using. This is essential for monitoring and understanding the performance of each container.
    
    LXC uses cgroups to manage the container's access to hardware resources, such as:
    *   **CPU**: Limiting CPU time or pinning the container to specific CPU cores.
    *   **Memory**: Setting limits on memory consumption.
    *   **Block I/O**: Limiting read/write rates to storage devices.

---

## Benefits and Use Cases

### Benefits
- **Performance**: Because it shares the host kernel and avoids hardware emulation, LXC offers near-native performance.
- **Density & Efficiency**: More lightweight than full VMs, allowing for higher density on a single physical host.
- **Full OS Environment**: Provides a complete and persistent Linux environment, which is ideal for development, testing complex multi-process applications, or running legacy software that expects a traditional OS.
- **Flexibility**: Offers more control over the container's configuration (networking, storage) compared to higher-level tools like Docker.

### Use Cases
- **Lightweight VM Replacement**: For scenarios where you need the isolation and full OS environment of a VM but without the overhead of a hypervisor.
- **Development & Testing Environments**: Quickly spin up isolated, full-featured Linux environments for development or for running integration tests.
- **Hosting Legacy Applications**: Migrating older applications that are not designed to run as a single process in an application container.
- **Network Simulation**: Creating complex virtual network topologies for testing or training purposes.

---

## Basic Commands

Here are a few examples of basic `lxc` commands to give a feel for how it operates.

```bash
# Create a new Ubuntu 20.04 container named "my-ubuntu"
lxc-create -n my-ubuntu -t ubuntu -- -r focal

# Start the container
lxc-start -n my-ubuntu -d

# List all containers
lxc-ls -f

# Get a shell inside the running container
lxc-attach -n my-ubuntu

# Stop the container
lxc-stop -n my-ubuntu
```

## Related Concepts

- **[[containerization]]**: The overarching concept of OS-level virtualization.
- **[[docker|Docker]]**: A higher-level container platform that originally used LXC as its backend before developing its own runtime (`libcontainer`).
- **[[kubernetes|Kubernetes]]**: While Kubernetes is primarily designed to orchestrate Docker-compatible (application) containers, it can also manage system containers with the right configuration, though this is not a common use case.

---

## Resources & links

### Articles

1.  **[LXC - Official Website](https://linuxcontainers.org/lxc/introduction/)**

    The official introduction from the Linux Containers project. It explains LXC's core mission to provide an environment as close as possible to a standard Linux installation but without the need for a separate kernel. It details the key security features (namespaces, cgroups) and provides an overview of the toolset.

2.  **[LXC vs. Docker: Which One Should You Use? - Docker Blog](https://www.docker.com/blog/lxc-vs-docker/)**

    This article from the official Docker blog provides a balanced comparison of the two technologies. It highlights that while Docker was initially built on LXC, it evolved to focus on application containerization, whereas LXC is geared towards providing a full, lightweight OS environment. It's a great resource for understanding the different philosophies and use cases.

3.  **[A Brief Introduction to LXC Containers - John Ramsden](https://ramsdenj.com/posts/2016-11-23-a-brief-introduction-to-lxc-containers/)**

    This blog post offers a practical, hands-on introduction to getting started with LXC. It covers creating containers from templates, managing them with `lxc-*` commands, and provides a clear, simple guide for developers new to the technology.

### Videos

1.  **[Proxmox LXC - How To Guide - Better Than A VM? - Learn DevOps](https://www.youtube.com/watch?v=xKhWRMj5Nrc)**

    This video provides a practical guide on creating and managing LXC containers within a Proxmox environment. It walks through the process of deploying applications like PiHole inside an LXC container and discusses the performance and resource benefits compared to using a full virtual machine.

2.  **[What are LXC Containers? (And how they're different from Docker) - LearnLinuxTV](https://www.youtube.com/watch?v=_KnmRdK69qM)**

    This video from LearnLinuxTV offers a clear explanation of what LXC containers are, focusing on their role as "system containers" that behave like lightweight VMs. It effectively contrasts this with Docker's "application container" model, helping viewers understand the distinct use cases for each technology.
