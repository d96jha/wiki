
Thanks — that helps a lot. Alpine is minimal and uses **musl libc** instead of glibc, so kernel module development is a bit trickier, but still doable.

---

## ✅ Fixing `build: No such file or directory` on **Alpine Linux**

### ❓ Why this happens

On Alpine, `/lib/modules/$(uname -r)/build` is **not created by default**, because Alpine doesn’t install kernel headers or sources unless you explicitly add them.

---

## ✅ Solution: Install Kernel Headers for Alpine

### 🔸 Step 1: Enable community repo (if not already enabled)

Edit `/etc/apk/repositories` and ensure the community repo is **uncommented**, e.g.:

```
http://dl-cdn.alpinelinux.org/alpine/v3.19/community
```

Match `v3.19` to your Alpine version (`cat /etc/alpine-release`).

---

### 🔸 Step 2: Install the right kernel headers

Run:

```bash
apk update
apk add linux-headers linux-lts-dev build-base
```

This installs:

* `linux-headers`: generic kernel headers (userspace)
* `linux-lts-dev`: **kernel build system and source headers**
* `build-base`: GCC, make, etc.

If you're using a **non-LTS kernel** (e.g. edge or custom), use:

```bash
apk add linux-edge-dev
```

Or:

```bash
apk search linux-*dev
```

To list options.

---

### 🔸 Step 3: Point to correct kernel build directory

Alpine doesn’t populate `/lib/modules/$(uname -r)/build`, so you'll need to **explicitly set the path to kernel sources**.

#### Modify your `Makefile` like this:

```makefile
KDIR := /usr/src/linux-headers-$(uname -r)

obj-m += simple_module.o

all:
	make -C $(KDIR) M=$(PWD) modules

clean:
	make -C $(KDIR) M=$(PWD) clean
```

> Replace `$(uname -r)` with your actual kernel version if that path doesn’t work (e.g. `linux-headers-6.6.21` or similar).

You can find installed headers with:

```bash
ls /usr/src/
```

---

### 🔸 Step 4: Build the module

```bash
make
```

---

### 🔸 Step 5: Load the module

```bash
sudo insmod simple_module.ko
dmesg | tail
```

---

## 🧼 Optional: Symlink `build` directory

To make the standard Makefile (without custom KDIR) work:

```bash
sudo mkdir -p /lib/modules/$(uname -r)
sudo ln -s /usr/src/linux-headers-$(uname -r) /lib/modules/$(uname -r)/build
```

---

## Let Me Know:

* Your exact Alpine version: `cat /etc/alpine-release`
* Your kernel version: `uname -r`

And I’ll give you the exact `apk add` command and paths.
