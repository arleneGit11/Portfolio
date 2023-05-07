# Monitoring a GlusterFS Volume

## Introduction

A customer has asked you to test out the Gluster `volume top` and `volume profile` commands within their GlusterFS cluster. The customer needs you to run custom scripts to create some files on their GlusterFS volume. Once the files are created, they want you to compare GlusterFS profile data to make sure it is working properly. In addition to testing out GlusterFS profile, they would also like you to run two Gluster top performance tests to ensure they are working as well. Upon completion of this lab, you will know how to use Gluster Volume Profile and run Gluster top performance tests.

## Solution

1. Log in to `server-1` server using the credentials provided:
   ```
   ssh cloud_user@<PUBLIC_IP_ADDRESS>
   ```

1. Switch to the root user:
   ```
   sudo su -
   ```

1. Enter the `cloud_user` password.

1. Clear your screen:
   ```
   clear
   ```

### Start GlusterFS Profile

Start the Gluster profile on the `gfs_vol` volume using the `gluster volume profile gfs_vol start` command.

### Compare GlusterFS Profile Data

1. Run the first script (usually takes 20-25 seconds):
   ```
   ./gen_files-1M.sh
   ```

1. Verify the new files were created in `/mnt`:
   ```
   ls /mnt
   ```

1. Collect the profile data, and save it to a file named `profile-1.txt`:
   ```
   gluster volume profile gfs_vol info > profile-1.txt
   ```

1. Verify that the profile data is in the file:
   ```
   less profile-1.txt
   ```

1. Press `q` to go back to the terminal.

1. Run the second script (usually takes 5-10 seconds):
   ```
   ./gen_files-256.sh
   ```

1. Verify the new files were created in `/mnt`:
   ```
   ls /mnt
   ```

1. Save the profile data to a file named `profile-2.txt`:
   ```
   gluster volume profile gfs_vol info > profile-2.txt
   ```

1. Verify that the profile data is in the `profile-2.txt` file:
   ```
   less profile-2.txt
   ```

1. Press `q` to go back to the terminal.

1. Compare the two files by using the `sdiff` command for a side-by-side comparison:
   ```
   sdiff profile-1.txt profile-2.txt  | less
   ```

1. Press `q` to go back to the terminal.

1. Stop profiling:
   ```
   gluster volume profile gfs_vol stop
   ```

     > - **Tip:** You can also use the `reverse-i-search` function by finding the Gluster profile start command and changing `start` to `stop`:
      > - Press **Ctrl**+**r**.
      > - Start typing the word `start`.
      > - Press the right arrow and change `start` to `stop`.


### Run GlusterFS Top Performance Tests

1. Run the `write-perf` test (usually takes about 10 seconds):
   ```
   gluster volume top gfs_vol write-perf bs 512 count 1000000 list-cnt 5
   ```

1. Run the `read-perf` test (usually takes about 10 seconds):
   ```
   gluster volume top gfs_vol read-perf bs 256 count 1000000 list-cnt 5
   ```

## Conclusion

Congratulations — you've completed this hands-on lab!
