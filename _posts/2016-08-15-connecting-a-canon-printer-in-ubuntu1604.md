---
layout: post
title: Installing a Canon LBP7010C printer in Ubuntu 16.04
description: "how to connect and install a canon LBP7010C printer in Ubuntu 16.04"
modified: 2016-08-15
tags: [Ubuntu]
categories: [Ubuntu]
share: false
---

I struggled with this for a while, and I think the main trouble I had was with selecting the correct PPD file for CUPS. These commands mainly came from [this Ubuntu-fr guide](https://doc.ubuntu-fr.org/imprimante_canon_capt2).

1. Get the [Linux drivers](http://www.canon-europe.com/support/consumer_products/products/printers/laser/i-sensys_lbp7010c.aspx?type=drivers&language=EN&os=Linux%20(64-bit)) from Canon. (These are for LBP7010C.)
2. Extract the archive:
   ```shell
   $ tar xvzf Linux_CAPT_PrinterDriver_V270_uk_EN.tar.gz
   ```
3. The guide mentioned above suggests that Glade is a dependency:
   {% highlight shell%}
   $ sudo apt-get install libglade2-0
   {% endhighlight %}
4. Before installing the drivers, install these 32-bit packages:
   {% highlight shell%}
   $ sudo apt-get install libatk1.0-0:i386 libcairo2:i386 libgtk2.0-0:i386 libpango1.0-0:i386 libstdc++6:i386 libxml2:i386 libpopt0:i386
   {% endhighlight %}
5. Navigate to the folder containing the Debian package (64-bit in my case) and install the common and capt drivers:
   {% highlight shell%}
   $ sudo dpkg -i cndrvcups-common_3.20-1_amd64.deb cndrvcups-capt_2.70-1_amd64.deb
   {% endhighlight %}
6. Now install the driver in CUPS:
   {% highlight shell%}
   $ sudo /usr/sbin/lpadmin -p LBP7010C-7018C -m CNCUPSLBP7010CCAPTJ.ppd -v ccp://localhost:59787 -E
   {% endhighlight %}
   Note that I'm using the CAPTJ driver rather than the CAPTK (CNCUPSLBP7018CCAPTK.ppd) driver recommended in the Canon README file - it refused to work when I tried the CAPTK driver.
7. Now add the printer to ccpd (the printer daemon for CUPS): 
   {% highlight shell%}
   $ sudo /usr/sbin/ccpdadmin -p LBP7010C-7018C -o /dev/usb/lp2
   {% endhighlight %}
   You'll need to check the USB connection to the printer before running this:
   {% highlight shell%}
   $ lsusb
   {% endhighlight %}
   to make sure it is connected, and:
   {% highlight shell%}
   $ ls -l /dev/usb/lp* /dev/bus/usb/*/*
   {% endhighlight %}
   you can also check if CUPS detects the printer:
   {% highlight shell%}
   lpinfo -v
   {% endhighlight %}
8. The guide also suggests the following addition to /etc/init.d/ccpd directly after the first two lines:
   {% highlight shell%}
   ### BEGIN INIT INFO
   # Provides: ccpd
   # Required-Start: $ local_fs remote_fs $ $ $ $ network syslog named
   # Should-Start: $ ALL
   # Required-Stop: $ syslog $ remote_fs
   # Default-Start: 2 3 4 5
   # Default-Stop: 0 1 6
   # Description: Start Canon Printer Daemon for CUPS
   ### END INIT INFO
   {% endhighlight %}
9. It is also worth checking that the device path is correctly defined in /etc/ccpd.conf:
   {% highlight shell%}
   <Printer LBP7010C-7018C>
   DevicePath /dev/usb/lp2
   </Printer>
   {% endhighlight %}
10. And that ccpdadmin has correctly verified the record:
   {% highlight shell%}
   $ sudo ccpdadmin
   {% endhighlight %}
11. Finish by starting ccpd:
   {% highlight shell%}
   $ sudo ccpd service start
   {% endhighlight %}
12. You can check the status of the printer using:
   {% highlight shell%}
   $ captstatusui -P LBP7010C-7018C
   {% endhighlight %}

It is also worth noting that the USB connection to my printer only works when I restart the printer (i.e. if the printer is on and the computer is restarted the USB connection to the printer is not picked up).