### Reason for requirement
* While performing System format, Pendrive partitioned to multiple drives. Which had to be clubbed to one

### Commands to club PenDrive's partition to one and create primary partition 
> diskpart

> list disk

> select Disk 1

> clean

> create partition primary
