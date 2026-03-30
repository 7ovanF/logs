Yes—there *is* a precise way to do it. You do **not** need to guess sizes.

The key idea is:

> `ntfsresize` determines the *exact minimum size*, and you align the partition boundary to that value.

---

## 1. What `ntfsresize` actually gives you

Run:

```bash
ntfsresize --info /dev/sdXn
```

You’ll see something like:

* **Current volume size**
* **Minimum size**

That **minimum size is the exact lower bound** you can safely shrink to.

You can also simulate a shrink:

```bash
ntfsresize --no-action --size 80G /dev/sdXn
```

This validates without touching data.

---

## 2. Converting that into a partition boundary (important step)

Partition tools (`fdisk`, `parted`) work in:

* sectors
* or MiB/GiB (depending on settings)

So you don’t input “random numbers”—you align to one of these:

### Option A — Use MiB/GiB units (simplest)

In `parted`:

```bash
(parted) unit MiB
(parted) print
```

Then:

```bash
(parted) resizepart N 81920MiB
```

(81920 MiB = 80 GiB, matching your `ntfsresize` target)

---

### Option B — Exact sector alignment (more precise)

1. Get sector size:

   ```bash
   blockdev --getss /dev/sdX
   ```

2. Convert size → sectors:

   ```
   sectors = size_in_bytes / sector_size
   ```

3. Then in `fdisk`:

   * delete partition
   * recreate with same start sector
   * set **end sector exactly**

This is how you avoid any ambiguity.

---

## 3. Cleaner method: let `ntfsresize` decide everything

You can let it compute optimal size:

```bash
ntfsresize --info /dev/sdXn
```

Look for:

* **"You might resize at X bytes"**

Then directly match that value in `parted`.

---

## 4. Why tools don’t “auto-trim” the partition

Because:

* **Filesystem ≠ partition**
* `ntfsresize` has no authority to change partition tables
* `parted` has no understanding of NTFS internals

So the process is intentionally **manual but deterministic**

---

## 5. Subtle but important detail

After shrinking:

* There will be **unused space at the end of the partition**
* That space is *invisible* to the filesystem but still inside the partition

Your job with `parted`:

> Move the partition boundary inward to match the filesystem

---

## 6. Practical workflow (clean, no guessing)

1. Check minimum:

   ```bash
   ntfsresize --info /dev/sdXn
   ```

2. Shrink filesystem:

   ```bash
   ntfsresize --size 80G /dev/sdXn
   ```

3. Resize partition precisely:

   ```bash
   parted /dev/sdX
   (parted) unit GiB
   (parted) resizepart N 80
   ```

4. Final consistency check:

   ```bash
   ntfsresize --fix /dev/sdXn
   ```

---

## 7. Key takeaway (conceptual clarity)

There is no “trim leftover space” command because:

* Filesystem shrink creates **logical free space**
* Partition tools must **explicitly redefine physical boundaries**

But the process is still exact—not approximate—because:

> `ntfsresize` provides the exact safe limit, and you map that 1:1 to partition size.

---

If you want, I can go deeper into **how GPT/MBR store partition end sectors and why alignment (1 MiB boundaries) matters for SSD performance and correctness**—that’s the next layer of precision here.
