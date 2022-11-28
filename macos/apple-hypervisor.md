# apple hypervisor

To determine if the machine has apple's hypervisor support for virtualization

```
sysctl kern.hv_support

```

## further reading

The Hypervisor framework has the following requirements:

### Supported hardware

    The Hypervisor framework requires hardware support to virtualize hardware resources. On Apple silicon, that includes the Virtualization Extensions. On Intel-based Mac computers, the framework supports machines with an Intel VT-x feature set that includes Extended Page Tables (EPT) and Unrestricted Mode.

    At runtime, determine whether the Hypervisor APIs are available on a particular machine with the sysctl command, passing kern.hv_support as an argument.

### Entitlements

    All process must have the com.apple.security.hypervisor entitlement to use Hypervisor API.

source: https://developer.apple.com/documentation/hypervisor

