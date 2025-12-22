---
layout: project
title: Maestro
display_order: 3
description: A hobby operating system for the x86 architecture.
tech: C, x86 ASM
image: /assets/maestro.png
links:
  - name: "GitHub"
    url: "https://github.com/samkravitz/maestro"
---

## Overview

My favorite class during my undergrad was Operating Systems. In the class, we added and tinkered with features for a toy operating system, learning basics about task management, memory management, locking mechanisms, and much more. I really loved learning about it, and wanted to create my own kernel to learn more. Maestro is my symphony in C.

## Features

- Small, fast, UNIX-like 32-bit monolithic kernel
- EXT2 filesystem driver and executable format runner
- Handwritten bootloader in x86 assembly
- Hardware (keyboard, mouse, monitor) support
- Handwritten libc

## Technical Details

Maestro implements the EXT2 filesystem and ELF format for executables. There is a usermode and kernel mode, and it's own shell that is run in user mode. There is the beginnings of a network stack and graphical output, but I have not gotten very far in these areas yet.
