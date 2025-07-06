### 𝗨𝗻𝗱𝗲𝗿𝘀𝘁𝗮𝗻𝗱𝗶𝗻𝗴 𝗛𝗮𝗿𝗱 𝗟𝗶𝗻𝗸𝘀 𝗶𝗻 𝗟𝗶𝗻𝘂𝘅 – 𝗣𝗮𝗿𝘁 𝟭

A hard link is a `filename` that points directly to an `inode` on the disk. It allows you to create multiple filenames that reference the same inode and share the same underlying data. Any changes made through one hard link are reflected in all others, as they all point to the same file content. 

Think of a house (inode) with multiple doors or entry points (filenames). No matter what door you enter from, you will find the same furniture inside. The data lives in the house (inode), not in the doors (filenames). So, the filename itself holds no data, it simply points to where the data lives. I covered inodes in a previous post feel free to check it out if you’d like a deeper look at how file metadata works in Linux.

Why does this matter for DevOps/Cloud Engineers? Creating a hard link gives you a backup reference to critical data without duplicating the file or wasting disk space. It’s especially useful during log rotation, safe deletions, and scenarios where you need to inspect logs, maintain backups, or run tests all while pointing to the same underlying data.

𝗛𝗲𝗿𝗲'𝘀 𝗵𝗼𝘄 𝗶𝘁 𝘄𝗼𝗿𝗸𝘀 𝗶𝗻 𝗽𝗿𝗮𝗰𝘁𝗶𝗰𝗲 𝗮𝗻𝗱 𝗜'𝘃𝗲 𝗶𝗻𝗰𝗹𝘂𝗱𝗲𝗱 𝗮 𝘁𝗲𝗿𝗺𝗶𝗻𝗮𝗹 𝘀𝗰𝗿𝗲𝗲𝗻𝘀𝗵𝗼𝘁 𝗯𝗲𝗹𝗼𝘄 𝘁𝗼 𝘀𝗵𝗼𝘄 𝗶𝘁 𝗶𝗻 𝗮𝗰𝘁𝗶𝗼𝗻:

1. Create a filename and add some content: 
    ```bash
    echo "Greetings!" > demo.log
    ```

 2. List the file with inode information:
    ```bash
    ls -li
    ```

 3. Create a hard link named demo-backup.log
    ```bash
    ln demo.log demo-backup.log
    ```

 4. Check both filenames now point to the same inode (𝟵𝟱𝟭𝟭𝟬𝟱):
    ```bash
    ls -li
    ```

 5. Overwrite content via the second filename:
    ```bash
    echo "Welcome!" > demo-backup.log
    ```

 6. Check contents – both filenames reflect the updated content:
    ```bash
    cat demo.log
    cat demo-backup.log
    output for both files: Welcome!
    ```

 7. Delete one of the filenames:
    ```bash
    rm demo.log
    ```

 8. Verify the remaining filename still works: 
    ```bash
    ls
    cat demo-backup.log
    ```