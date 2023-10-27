# Errata: Remote Home Directory

## SELinux Configuration

The presenter did not correct the SELinux fcontext labels for the new base directory, `/rhome`, on the server machine.

This does not immediately pose a problem in the video but would cause some SELinux targeted apps to malfunction under certain use cases (`OpenSSH` for example will not be able to read authorized keys properly).

### The Fix:

#### 1. Edit `semanage.conf`:

Edit the `/etc/selinux/semanage.conf` file to allow `semanage` to scan `/etc/passwd` (which works with LDAP) and correctly label home directories in non-default locations.

```bash
sudo vim /etc/selinux/semanage.conf
```

Find the line that starts with `usepasswd` and set its value to `true`.

```ini
usepasswd=true
```

Save and exit the text editor.

#### 2. Set `/rhome` Labels:

Now, set the SELinux labels for `/rhome` to be the same as those for `/home`:

```bash
sudo semanage fcontext -a -e /home /rhome
```

#### 3. Update the Labels:

Use the `restorecon` command to apply the context label changes recursively to `/rhome`:

```bash
sudo restorecon -R /rhome
```

#### Optional: Inspect Home Directory Template:

If you're curious about how `semanage` labels the contents of home directories, you can inspect the `homedir_template`:

```bash
cat /etc/selinux/targeted/modules/active/homedir_template
```
#### Sources:
[UnixMen Tutorial](https://www.unixmen.com/selinux-and-non-default-home-directory-locations/)