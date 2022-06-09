# using stacks to navigate

## `pushd` and `popd`

### go to dir1 and add it to the stack

```bash
pushd /path/dir1
```

### list the stack

```bash
dirs
```

### go to dir2 and add it to the stack

```bash
pushd /path/dir2
```

### go to dir3 and add it to the stack

```bash
pushd /path/dir3
```

### go to dir4 and add it to the stack

```bash
pushd /path/dir4
```

### go to dir5 and add it to the stack

```bash
pushd /path/dir5
```

### go to dir4 by popping dir5 out of the stack

```bash
popd
```

## list stack with `dirs`

### list the stack

```bash
dirs
```

### list the stack with index numbers

```bash
dirs -v
```

## Use with `cd`

### go to directory in index 2 of stack

```bash
cd ~2
```

### go to directory in index 4 of stack

```bash
cd ~4
```
