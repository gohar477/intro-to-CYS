# _**Introduction to Cyber Security Lab (Project)**_

## _**Privilege Escalation through SUID & SGID**_

## _**Team Members:**_
- **Sumaiya** Arshad
- Muhammad Abdur **Rafay**
- **Bareera** Khan
- Muhammad **Haris**

## _**Topics Overview**_
1. What are SUID & SGID?
2. Why they are used?
3. How to find them on a Server?
4. How to abuse them to elevate privileges?
5. How to patch them

## _**What are Special Permissions?**_

There are three special permissions:

Set User ID (SUID), Set Group ID (SGID) and the Sticky bit. These special permissions enhance security and control in multi-user environments, ensuring proper execution and protection of shared resources.

## _**SUID**_

### _***Purpose:***_

When a program with SUID is executed, it runs with the permissions of the owner of the program instead of the user who is running it.

### ***Example:***

Consider a program owned by the superuser (root) that regular users need to run but requires special privileges. By setting the **SUID** bit on the program, when a regular user runs it, the program temporarily gains the superuser's permissions for that specific execution.

```bash
$ ls -l my_program
-rwsr-xr-x 1 root users 12345 Dec 16 12:34 my_program
```
### ***To add this permission:***
    
```bash
chmod u+s
```

## _**SGID**_

### _***Purpose:***_

Similar to SUID, but it sets the group ownership of the executing process to the group that owns the file, rather than the group of the user who runs it.

### ***Example:***

Imagine a directory shared by a group of users where any file created within it should inherit the group ownership of the directory. Setting the SGID bit on the directory achieves this.

```bash
$ ls -ld shared_directory
drwxrwsr-x 2 usergroup users 4096 Dec 16 12:34 shared_directory
```
### ***To add this permission:***
    
```bash
chmod g+s
```

## _**Sticky**_

### _***Purpose:***_

It is commonly used on directories to restrict the deletion of files within that directory to only the file owner. It is often applied to directories like /tmp to ensure that users can only delete their own files.

### ***Example:***

Consider a directory where multiple users can write files. Without the sticky bit, any user might be able to delete files owned by others. With the sticky bit, only the file owner can delete their own files.

```bash
$ ls -ld my_directory
drwxrwxrwt 2 user1 users 4096 Dec 16 12:34 my_directory
```
### ***To add this permission:***
    
```bash
chmod o+t
```

## _**Important Note:**_

In the examples above, the letters '**s**' and '**t**' in the permission string indicate the presence of SUID and Sticky bits, respectively. '**s**' indicates both execute permission and the SUID or SGID bit, while '**t**' indicates the Sticky bit. The uppercase '**S**' or '**T**' indicates the absence of execute permission for the owner or group, respectively.

## _**How to find them on a Server?**_

```bash
find / -perm -4000 2>/dev/null
```
```bash
find / -perm -u=s 2>/dev/null
```

## _**How to abuse them to elevate privileges?**_

<p align="left">
  Video Link <a href="https://github.com/DenialArcus/Intro-to-CYS-Lab/blob/main/demo_video%20.mp4" style="text-decoration: none; color: inherit; font-weight: bold;">Here</a>
</p>

## _**How to Avoid it ??**_

1. _***Minimize SUID Programs:***_

   Limit the number of programs with the SUID bit set. Only essential programs that require elevated privileges should have this permission.

2. _**Regular Auditing:**_

   Regularly audit the system for SUID programs. Use tools like find to identify files with the SUID bit set and review their necessity.
   ```bash
   find / -type f -perm -4000
   ```

3. _**Use Capabilities Instead:**_

   Instead of relying on SUID, consider using capabilities. Capabilities allow fine-grained control over specific privileges without granting full root access.

4. _**Use AppArmor or SELinux:**_

   Mandatory Access Control (MAC) frameworks like AppArmor or SELinux can help restrict the actions of SUID programs, providing an additional layer of security.

6. _**Regularly Update Software:**_

   Keep your software and system up-to-date. Developers often release patches for security vulnerabilities, and applying updates promptly can mitigate
   potential risks.

7. _**Implement the Principle of Least Privilege (PoLP):**_

   Ensure that users and processes have the minimum level of access necessary to perform their tasks. This reduces the potential impact of privilege escalation.

8. _**Monitor File Integrity:**_

   Use tools like Tripwire or AIDE to monitor file integrity. Any unexpected changes to SUID binaries should be investigated.

9. _**Regular Security Audits:**_

   Conduct regular security audits to identify and address potential vulnerabilities. Automated tools and manual inspections can be used for this purpose.

10. _**User Education:**_

    Educate users about the risks associated with SUID programs and the importance of not running unknown or untrusted executables.

10. _**Logging and Monitoring:**_

    Enable logging for system activities and monitor for any suspicious activities related to SUID programs. This can help in identifying potential security breaches.

11. _**Periodic Access Reviews:**_

    Periodically review and update access permissions. Remove unnecessary SUID permissions from files or users who no longer require them.

12. _**Restrict SUID to Trusted Users:**_

    Limit the users who have the ability to set the SUID bit on files. This should be restricted to trusted administrators only.


## _**Slides**_

<p align="left">
  Presentation Slides Link <a href="https://www.canva.com/design/DAF3FIq5v8w/IryxNG9Xy-jQZDPRP0HIXQ/view?utm_content=DAF3FIq5v8w&utm_campaign=designshare&utm_medium=link&utm_source=editor" style="text-decoration: none; color: inherit; font-weight: bold;">Here</a>
</p>
