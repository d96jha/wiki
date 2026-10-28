
# simplest example linux kernel module implementation

Here's the **simplest possible Linux kernel module** (LKM) that you can write. This module does nothing fancy—just logs messages when it's loaded and unloaded.

---

### 📁 1. Create the module source file

Create a file named `simple_module.c`:

```c
#include <linux/init.h>      // Needed for the macros
#include <linux/module.h>    // Core header for loading LKMs
#include <linux/kernel.h>    // Contains types, macros, functions for the kernel

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Your Name");
MODULE_DESCRIPTION("A simple Linux kernel module");
MODULE_VERSION("0.1");

// Function called when module is loaded
static int __init simple_init(void) {
    printk(KERN_INFO "SimpleModule: loaded\n");
    return 0;
}

// Function called when module is removed
static void __exit simple_exit(void) {
    printk(KERN_INFO "SimpleModule: unloaded\n");
}

module_init(simple_init);
module_exit(simple_exit);
```

---

### 🛠️ 2. Create a Makefile

Create a file named `Makefile` in the same directory:

```makefile
obj-m += simple_module.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

---

### 🧪 3. Build the module

In your terminal:

```bash
make
```

This will generate a `simple_module.ko` file (the kernel object).

---

### 🚀 4. Insert the module

```bash
sudo insmod simple_module.ko
```

Check kernel messages:

```bash
dmesg | tail
```

You should see:

```
SimpleModule: loaded
```

---

### 🧹 5. Remove the module

```bash
sudo rmmod simple_module
```

Again, check logs:

```bash
dmesg | tail
```

You should see:

```
SimpleModule: unloaded
```

---

### ✅ Requirements

* You need the kernel headers installed:

  ```bash
  sudo apt install linux-headers-$(uname -r)
  ```
* Compile with `make` as shown above.

---

Let me know if you want to add parameters, interact with hardware, or hook into system calls.
