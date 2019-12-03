# Installing BigConnect

### Download installation kit

The installation kit comes in the form of a `.tgz` archive that has all components.

```bash
$ wget https://bits.bigconnect.io/downloads/bdl-<A.B>-<os>.tgz
```

`<A.B>` is the release version of BigConnect.

`<os>` is one of the supported platforms: _rhel, sles, debian, ubuntu_

**Example:**

```bash
$ wget https://bits.bigconnect.io/downloads/bdl-4.1-rhel.tgz
```

### Extract the archive

The archive **must** be extracted in the `/opt` directory. 

```bash
$ tar zxf bdl-<A.B>-<os>.tgz -C /opt/
```



