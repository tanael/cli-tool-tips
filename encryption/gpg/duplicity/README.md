# duplicity - encrypted bandwidth-efficient backup

## incremental with duplicity
```bash
duplicity --encrypt-sign-key nanthonypillai@olfeo.com Documents file://docsbackup
```

Run the command again after modifying the source folder, it will be
incremental.

## restore

```bash
duplicity file://docsbackup docsrestore
```

## restore of 3 days ago

```bash
duplicity --time 3D file://docsbackup docsrestore
```

## restore of 3 days old version of a specific file

```bash
duplicity --time 3D --file-to-restore private/eff.txt file://docsbackup docsrestore
```
