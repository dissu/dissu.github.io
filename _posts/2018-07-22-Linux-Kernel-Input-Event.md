# Linux Kernel Input Event

![Image of Linux Input Event](http://www.embeddedlinux.org.cn/essentiallinuxdevicedrivers/final/images/YTNyaWQ3ODBzOS9jL2VnbXRwNjQ5NWEzMi9yZzE1ZmkwcGdmaS4wLzFoaWc3c2M-.jpg)

## Background
### Architecture
 Input subsystem a collection of drivers that is degigned to support all the devices under Linux.
 Most of device drivers reside in drivers/input, although quite a few line in drivers/hid and drivers/platform

#### Device drivers
 Theses modules talk to the hardware, and provides events to the input module

#### Event Handlers
 These modules get event from input core and pass them where needed via various interface.
```
Driver → InputCore → Event Handler → User Space (user application)

```

##### udev

udev(userspace /dev) is a device manager for the Linux kernel. As the successor of devfsd and hotplug, udev primarily manages device nodes in the /dev/ directory. At the same time, udev also handles all user space events raised when hardware devices are added into the system or removed from it, including firmware loading as required by certain devices.

##### evdev

Generic input event interface in the Linux kernel. It generalizes raw input events from devices drivers and makes them available through chracter devices in the /dev/input/ directory. user-space library for the Linux kernel component evdev is called libevdev. Libevdev abstracts the evdev ioctls through type-safe interfaces and provides functions to change the appearance of the device.  Libevdev shares similarites with the read system call

For Weston/Wayland, the stack would look like this:
```
Linux kernel → libevdev → libinput → Weston → Wayland client
```


#### Device file
 In Unix-like operating systems, a device file or special file is an interface to a device driver that appears in a file system as if it were an ordinary file. There special files allow an application program to interact with a device by using its device driver via standard input/output system calls

##### Character devices

Character special files or character devices provide unbuffered, direct access to the hardware device. They do not necessarily allow programs to read or write single characters at a time; that is up to the device in question. The character device for a hard disk, for example, will normally require that all reads and writes are aligned to block boundaries and most certainly will not allow reading a single byte.

Character devices are sometimes known as raw devices to avoid the confusion surrounding the fact that a character device for a piece of block-based hardware will typically require programs to read and write aligned blocks.

### Event interface
You can use blocking and nonblocking reads, and also select() on the /dev/input/eventX devices, and you’ll always get a whole number of input events on a read. Their layout is:
```
struct input_event {
        struct timeval time;
        unsigned short type;
        unsigned short code;
        unsigned int value;
};
```
_ _ _
## Code
