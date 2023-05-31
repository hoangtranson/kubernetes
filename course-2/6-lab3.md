# Introduction to Kubestr

## Kubestr

Kubestr is a collection of tools to discover, validate and evaluate your kubernetes storage options.

As adoption of Kubernetes grows so have the persistent storage offerings that are available to users. The introduction of CSI (Container Storage Interface) has enabled storage providers to develop drivers with ease. In fact there are around a 100 different CSI drivers available today. Along with the existing in-tree providers, these options can make choosing the right storage difficult.

Kubestr can assist in the following ways:

- Identify the various storage options present in a cluster.
- Validate if the storage options are configured correctly.
- Evaluate the storage using common benchmarking tools like FIO.

## Using Kubestr

To install the tool

Download the latest release:

```
curl -sLO https://github.com/kastenhq/kubestr/releases/download/v0.4.17/kubestr-v0.4.17-linux-amd64.tar.gz
```

Unpack the tool and make it an executable chmod +x kubestr.

```
tar -xvzf kubestr-v0.4.17-linux-amd64.tar.gz -C /usr/local/bin
chmod +x /usr/local/bin/kubestr
```

To discover available storage options: Run kubestr

To run an FIO test

Using kubestr we can test sequential read and write speeds which are expected to be high.

Random reads and writes can be tested as well. Moreover, random writes separates good drives from the bad ones.

Copy the test specification:

```
cat <<'EOF'>ssd-test.fio
[global]
bs=4k
ioengine=libaio
iodepth=1
size=1g
direct=1
runtime=10
directory=/
filename=ssd.test.file

[seq-read]
rw=read
stonewall

[rand-read]
rw=randread
stonewall

[seq-write]
rw=write
stonewall

[rand-write]
rw=randwrite
stonewall
EOF
```

Run the following command to start the test

```
kubestr fio -f ssd-test.fio -s local-path
```

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/41f2ac6d-6cd8-4e47-bc7a-8d3dee689720)

Additional options like --size and --fiofile can be specified.

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/dcbbe63f-0678-4cd4-a1d3-7b65bc141540)

