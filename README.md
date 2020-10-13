# shrinkRPImages - Shrink Raspberry PI SD Images

This repository contains a utility useful for shrinking Raspbian images on SD cards.  I am often making appliances that are relatively small and while testing I often want to be able to clone images very quickly.  Once I have image that I want as a baseline or a backup, I copy the image to a mass storage device and then I use this utility to compact it into a smaller format.  This is done by using resize2fs and sfdisk to resize the file system and resize the partitions.  Once that resizing is done, truncate eliminates the unused portion of the file.  So an image that started out as 16G can be trimmed down the 2.5 or 3G - it all depends on what you have on your SD card.

The smaller image can then be copied to another card and you can boot a PI off that "new" card.  The first boot of that image will expand the second/root file system to consume the whole device.

Installation & Use

  1)  Clone this repository
      - git clone https://github.com/mbroihier/shrinkRPImage
  2)  Run the utility on an image file
      - cd shrinkRPIImage
      - sudo ./shrinkRPImage myRPI.img

My workflow is usually something like this:
   - I copy an sd card to my mass storage device
     + sudo dd bs=4M status=progress if=/dev/sdc of=myRPI.img
   - I shrink the image
     + sudo ./shrinkRPImage myRPI.img
   - I copy it to another card
     + sudo dd bs=4M status=progress if=myRPI.img of=/dev/sdc

Of course, myRPI.img is any file name and /dev/sdc must point to where your (unmounted) Raspbian image is.

**** Note ****
This works only with Raspbian images (starting with buster) and the images are restricted to the following requirements:  a) there must only be two partions on the SD card (the first one FAT32 and the second Linux), b) you must not be using any loop devices prior to starting this utility, and c) you must have a mount point at /mnt and there must not be anything mounted to that mount point.

