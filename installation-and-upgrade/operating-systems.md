# Operating Systems

### Versions

| **Version** |  |
| :--- | :--- |
| **RHEL / CentOS \(x64\)** | 7.x |
| **Ubuntu \(x64\)** | 18.04 LTS \(Bionic Beaver\) 16.04 LTS \(Xenial\) |

### System Settings

#### Maximum Open Files

The recommended maximum number of open file descriptors is 10000, or more. To check the current value set for the maximum number of open file descriptors, execute the following shell commands on each host:

```text
ulimit -Sn
ulimit -Hn
```

If the output is not greater than 10000, run the following command to set it to a suitable default:

```text
ulimit -n 10000
```

