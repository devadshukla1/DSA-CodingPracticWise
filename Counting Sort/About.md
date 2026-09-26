# Counting Sort

Counting Sort is a **non-comparison-based sorting algorithm** that works by counting how many times each value occurs in the input array.

It is particularly efficient when the input consists of **non-negative integers within a relatively small range**.

---

## Conditions for Counting Sort

Counting Sort is most effective when the following conditions are satisfied.

### 1. Integer Values

Counting Sort works with **integer values** because each value is used as an index in the counting array.

For example:

```text
Input value:  3
Count index:  3
```

If `3` occurs three times, the counting array stores:

```text
countArray[3] = 3
```

Because values are directly mapped to array indices, Counting Sort is not naturally suited for floating-point or arbitrary non-integer values.

---

### 2. Non-Negative Values

The standard implementation of Counting Sort uses the input value as an index:

```python
count[value] += 1
```

For example:

```text
value = 3
count[3] += 1
```

This works because `3` is a valid array index.

However, consider a negative value:

```text
value = -3
count[-3] += 1
```

A normal counting array is indexed from `0`, so negative values cannot be directly used as indices.

> **Note:** Counting Sort *can* be adapted to handle negative integers by using an offset based on the minimum value. However, the standard implementation generally assumes non-negative integers.

---

### 3. Limited Range of Values

The range of possible values should be relatively small compared with the number of elements.

Let:

* `n` = number of elements in the input array
* `k` = range of values, typically `maxValue + 1` for non-negative integers

Counting Sort requires a counting array of size approximately `k`.

For example:

```text
Array:       [2, 3, 0, 2, 3, 2]
Number of elements (n) = 6
Maximum value          = 3
Counting array size    = 4
```

This is efficient because the range is small.

But consider:

```text
Array: [1, 1000000]
```

There are only `2` elements, but a standard counting array would need approximately `1,000,001` positions.

That would waste a large amount of memory.

Therefore, Counting Sort is most suitable when:

```text
k is reasonably small compared with n
```

---

## Summary of Conditions

| Condition           | Reason                                                  |
| ------------------- | ------------------------------------------------------- |
| Integer values      | Values are mapped to counting-array indices             |
| Non-negative values | Standard implementation uses values directly as indices |
| Small/limited range | A large range would require excessive memory            |

### Complexity

For Counting Sort:

```text
Time Complexity:  O(n + k)
Space Complexity: O(k)
```

Where:

* `n` = number of elements
* `k` = range of possible values

---

# Manual Walkthrough

Let's manually execute Counting Sort using the following array:

```text
myArray = [2, 3, 0, 2, 3, 2]
```

The values range from `0` to `3`, so we need a counting array with **4 positions**.

---

## Step 1 — Start With the Unsorted Array

```text
myArray = [2, 3, 0, 2, 3, 2]
```

---

## Step 2 — Create the Counting Array

We need positions for values:

```text
0, 1, 2, 3
```

Therefore:

```text
countArray = [0, 0, 0, 0]
```

The index represents the value, while the stored number represents its frequency.

```text
Index:       0  1  2  3
             ↓  ↓  ↓  ↓
countArray = [0, 0, 0, 0]
```

---

## Step 3 — Count the First Value

The first value is `2`.

Increase the count at index `2`:

```text
countArray[2] += 1
```

Result:

```text
myArray   = [2, 3, 0, 2, 3, 2]
countArray = [0, 0, 1, 0]
```

This means:

```text
2 → appears 1 time
```

---

## Step 4 — Count the Next Value

The next value is `3`.

Increase the count at index `3`:

```text
countArray[3] += 1
```

The value `3` can then be removed from the working array.

```text
myArray   = [3, 0, 2, 3, 2]
countArray = [0, 0, 1, 1]
```

Now:

```text
2 → 1 occurrence
3 → 1 occurrence
```

---

## Step 5 — Count `0`

The next value is `0`.

Increase the count at index `0`:

```text
countArray[0] += 1
```

Result:

```text
myArray   = [0, 2, 3, 2]
countArray = [1, 0, 1, 1]
```

Now:

```text
0 → 1 occurrence
2 → 1 occurrence
3 → 1 occurrence
```

---

## Step 6 — Finish Counting

Continue the same process until the input array becomes empty.

After counting every value:

```text
myArray   = []
countArray = [1, 0, 3, 2]
```

This tells us:

| Value | Frequency |
| ----: | --------: |
|   `0` |         1 |
|   `1` |         0 |
|   `2` |         3 |
|   `3` |         2 |

Therefore, the original array contains:

```text
0 → 1 time
1 → 0 times
2 → 3 times
3 → 2 times
```

---

# Reconstructing the Sorted Array

Now we use `countArray` to rebuild the original array in sorted order.

We process the counting array from **left to right**, which naturally gives us values from smallest to largest.

---

## Step 7 — Process Value `0`

```text
countArray[0] = 1
```

This means `0` occurs once.

Add one `0` to the array:

```text
myArray = [0]
```

Then decrease its count:

```text
countArray = [0, 0, 3, 2]
```

---

## Step 8 — Process Value `1`

```text
countArray[1] = 0
```

This means `1` does not occur in the original array.

Therefore, nothing is added.

```text
myArray = [0]
countArray = [0, 0, 3, 2]
```

---

## Step 9 — Process Value `2`

```text
countArray[2] = 3
```

This means `2` occurs three times.

Add three `2`s:

```text
myArray = [0, 2, 2, 2]
```

Decrease the count as each `2` is added:

```text
countArray = [0, 0, 0, 2]
```

---

## Step 10 — Process Value `3`

```text
countArray[3] = 2
```

This means `3` occurs twice.

Add two `3`s:

```text
myArray = [0, 2, 2, 2, 3, 3]
```

After adding both:

```text
countArray = [0, 0, 0, 0]
```

---

# Final Result

The original array:

```text
[2, 3, 0, 2, 3, 2]
```

has been sorted into:

```text
[0, 2, 2, 2, 3, 3]
```

---

## How Counting Sort Works

The complete process can be summarized in two phases:

### Phase 1 — Counting

Count how many times each value appears.

```text
Input:
[2, 3, 0, 2, 3, 2]

Count:
0 → 1
1 → 0
2 → 3
3 → 2
```

### Phase 2 — Reconstruction

Read the counting array from left to right and recreate the values according to their frequencies.

```text
0 → 1 time
1 → 0 times
2 → 3 times
3 → 2 times
```

Result:

```text
[0, 2, 2, 2, 3, 3]
```

---

## Key Idea to Remember

> **Counting Sort does not compare elements with each other. It counts how many times each value occurs and then reconstructs the array in sorted order.**

The core relationship is:

```text
Value → Counting Array Index
Frequency → Value Stored at That Index
```

For example:

```text
countArray[2] = 3
```

means:

```text
The value 2 occurs 3 times.
```

This is the fundamental idea behind Counting Sort.
