[Duplicacy quick-start (CLI) - Duplicacy Forum](https://forum.duplicacy.com/t/duplicacy-quick-start-cli/1101)

```
/ # /config/bin/duplicacy_linux_x64_3.2.3 info /mnt/disks/backuphdd -e
Enter the storage password:********************************
Compression level: 100
Average chunk size: 4194304
Maximum chunk size: 16777216
Minimum chunk size: 1048576
Chunk seed: 14076ce14c8bbee5b0e3871fc9fec83f1aeb5952c398caa998b96c1156ee3359
backup
creative
isos
media
unraid
/ # /config/bin/duplicacy_linux_x64_3.2.3 init -e -storage-name local media /mnt/disks/backuphdd
Enter storage password for /mnt/disks/backuphdd:********************************
The storage '/mnt/disks/backuphdd' has already been initialized
Compression level: 100
Average chunk size: 4194304
Maximum chunk size: 16777216
Minimum chunk size: 1048576
Chunk seed: 14076ce14c8bbee5b0e3871fc9fec83f1aeb5952c398caa998b96c1156ee3359
/ will be backed up to /mnt/disks/backuphdd with id media
/ # /config/bin/duplicacy_linux_x64_3.2.3 list -id media -storage local
Storage set to /mnt/disks/backuphdd
Enter storage password:********************************
Snapshot media revision 1 created at 2024-03-31 17:54 -hash
Snapshot media revision 2 created at 2024-04-01 13:29 
Snapshot media revision 3 created at 2024-04-01 19:55 
Snapshot media revision 4 created at 2024-04-01 23:17 
Snapshot media revision 5 created at 2024-04-02 23:27 
Snapshot media revision 6 created at 2024-04-04 01:29 
Snapshot media revision 7 created at 2024-04-11 02:59 
Snapshot media revision 8 created at 2024-04-12 00:30 

/backuproot/media/restore # /config/bin/duplicacy_linux_x64_3.2.3 restore -r 1 -storage local "books/*"
Storage set to /mnt/disks/backuphdd
Enter storage password:********************************
Loaded 1 include/exclude pattern(s)
Indexing /backuproot/media/restore
Parsing filter file /backuproot/media/restore/.duplicacy/filters
Loaded 0 include/exclude pattern(s)
Restoring /backuproot/media/restore to revision 1
Downloaded books/.DS_Store (6148)
Downloaded books/APE_1.3_MOBI_BookBaby.epub (16055010)
Downloaded books/O'Reilly Linux eBooks/bashcookbook.epub (3256082)
Downloaded books/O'Reilly Linux eBooks/classicshellscripting.epub (1760292)
Downloaded books/O'Reilly Linux eBooks/cybersecurityopswithbash.epub (2769819)
Downloaded books/O'Reilly Linux eBooks/effectiveawk.epub (1730616)
Downloaded books/O'Reilly Linux eBooks/greppocketref.epub (1159230)
Downloaded books/O'Reilly Linux eBooks/introducingregularexpressions.epub (4510055)
Downloaded books/O'Reilly Linux eBooks/learningthebashshell_3rdedition.epub (1565867)
Downloaded books/O'Reilly Linux eBooks/learningtheviandvimeditors_7thedition.epub (4727032)
Downloaded books/O'Reilly Linux eBooks/linuxdevicedrivers.epub (1960522)
Downloaded books/O'Reilly Linux eBooks/linuxinanutshell.epub (2872954)
Downloaded books/O'Reilly Linux eBooks/linuxobservabilitywithbpf.epub (2684881)
Downloaded books/O'Reilly Linux eBooks/linuxpocketguide3e.epub (2929032)
Downloaded books/O'Reilly Linux eBooks/linuxsystemprogramming.epub (1610506)
Downloaded books/O'Reilly Linux eBooks/masteringregularexpressions.epub (20783241)
Downloaded books/O'Reilly Linux eBooks/sedandawk.epub (1640398)
Downloaded books/O'Reilly Linux eBooks/unixpowertools.epub (3024739)
Restored /backuproot/media/restore to revision 1
Total running time: 00:00:02

```

The repository is the local directory that contains the data you wish to backup. When you initialize Duplicacy in a specific directory, that directory becomes the repository.