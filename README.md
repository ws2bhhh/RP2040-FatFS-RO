# RP2040-FatFS-RO
An read-only FatFS implement for RP2040 on-board flash.
# Define FS_OFFSET in ff.h first
FS_OFFSET = File_system_start_address - XIP_BASE
# If it mounts successfully but keeps failing to find the target file(FRESULT = 4, aka FR_NO_FILE)
Try to set the Volume Serial Number to 0 and remove the Volume Name before upload the image.
# Sample code below, to prevent unknown problems
```
#include "ff.h"

/* Recommended to be defined as a global variable */
FATFS FatFs;
FIL fil;           /* File object */
char buffer[4096]; /* Line buffer */
FRESULT fr;        /* FatFs return code */
UINT br;           /* How many bytes were read */

void setup()
{
}

void loop()
{
    /* Give a work area to the default drive */
    fr = f_mount(&FatFs, "", 0);

    /* Open the "0.bin" file */
    fr = f_open(&fil, "0.bin", FA_READ);

    /* Read the file */
    fr = f_read(&fil, buffer, f_size(&fil), &br);

    /* Close the file */
    f_close(&fil);

    /* Unmounted the drive */
    f_unmount("");
}
```
