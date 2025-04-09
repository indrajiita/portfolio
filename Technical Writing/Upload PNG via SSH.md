# Copying PNG Images From Local to Server via SSH

## Content

- [What is SSH?](#what-is-ssh)
- [Limitations](#limitations)
- [Before you start](#before-you-start)
- [Basic syntax](#basic-syntax)
- [Copy selected files](#copy-selected-files)
- [Copy entire directory](#copy-entire-directory)
- [Copy wildcard files](#copy-wildcard-files)
- [Related topics](#related-topics)

## What is SSH?<a id="what-is-ssh"></a>

SSH (Secure Shell Protocol) is a secure network protocol that creates a safe channel to transfer data. With SSH, you can do the following:

- Secure remote access to resources
- Execute commands remotely
- Automate file transfers

Sending files over SSH relies on the SCP (Secure Copy Protocol), which securely transfers files and folders between two machines. For more information about the syntax and options, consult the manual pages by running these commands:

- For SSH: `man ssh` 
- For SCP: `man scp`

## Limitations<a id="limitations"></a>

This article covers how to copy PNG files from a local host to a remote host, both running on Unix or Linux OS. For other cases, see [Related topics](#related-topics).

## Before you start<a id="before-you-start"></a>

- To be able to copy files, get at least read permission on the source file and write permission on the target system.
- Ensure that you have the password to authenticate on the remote host.
- Verify that the remote server is running an SSH service and is accessible over the network.

## Basic syntax<a id="basic-syntax"></a>

To specify the location of the source files, use the following syntax:

```
source_directory/file1 file2
```

- **source_directory** is the name of the directory containing the files.
- **file1**, **file2** are the names of the files to copy, separated by space.

To specify the location to which the source file will be copied, use the following syntax:

```
dest_user@dest_host:dest_directory/newfile1 newfile2
```

- **dest_user** is the name of the account on the destination machine.
- **dest_host** is the hostname of the machine to which the source files will be copied.
- **dest_directory** is the name of the directory to which the source files will be copied.
- **newfile1**, **newfile2** are the names of the copy files, separated by space.

Note:

- To copy files with the original names, omit the filenames from the destination location.
- After running the command, you will be prompted for the remote user password. Once you successfully authenticate, the file will be transferred.

## Copy selected files<a id="copy-selected-files"></a>

The command below copies the files `map.png` and `screenshot.png` from the directory `png_files` to the directory `/png_files_new` on the remote host `dest_host.com`. Also, it will rename the files to `map_copy.png` and `screenshot_copy.png` respectively.

```
scp /png_files/map.png dest_user@dest_host.com:/png_files_new/map_copy.png
scp /png_files/screenshot.png dest_user@dest_host.com:/png_files_new/screenshot_copy.png
```

## Copy entire directory<a id="copy-entire-directory"></a>

To copy a directory and all its files, use `scp` with the `-r` option for recursive. The command below will copy the entire directory `png_files` from the local machine to the remote host `dest_host.com`.

```
scp -r /png_files/ dest_user@dest_host.com:/png_files_new/png_files/
```

## Copy wildcard files<a id="#copy-wildcard-files"></a>

If you need to copy all PNG files from a local folder, you can use an asterisk ( * ) as a wildcard. The command below will copy all PNG files from the directory `images` to the directory `/images_new` on the remote host `dest_host.com`.

```
scp /images/*.png dest_user@dest_host.com:/images_new/
```

Note: Wildcards are interpreted by the shell. Ensure that the wildcard is not enclosed in quotes, as this will prevent it from being expanded.

## Related topics<a id="related-topics"></a>

|To learn this                       |See this   |
| ---------------------------------- | --------- |
|Transfer files between remote hosts |[SCP: Remote to Remote](https://www.google.ru/search?q=SCP+remote+to+remote)|
|Transfer files over 1 GB            |[SCP: Transferring Large Files](https://unix.stackexchange.com/questions/190537/transferring-large-8-gb-files-over-ssh)|
|Use SSP on Windows machines         |[How to Use SCP command on Windows](https://success.tanaza.com/s/article/How-to-use-SCP-command-on-Windows)|
