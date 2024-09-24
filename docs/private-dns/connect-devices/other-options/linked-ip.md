---
title: Linked IPs
sidebar_position: 3
---

## What are linked IPs and why it's useful

Not all devices can support encrypted DNS protocols. In this case, users should consider setting up unencrypted DNS.

You can use a **linked IP address**: in this setup, the service will consider all standard DNS queries coming from that IP address and for that specific "device. The only requirement for a linked IP address is that it must be a residential IP.

:::note

A **residential IP address** is assigned to a device connected to a residential ISP. It's usually tied to a physical location and given to individual homes or apartments. People use residential IP addresses for everyday online activities like browsing the web, sending emails, using social media, or streaming content.

:::

Sometimes, a residential IP address may already be in use, and if you try to connect to it, AdGuard DNS will prevent the connection.

![Linked IPv4 address *border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/connect/linked.png)

If that happens, please reach out to support at [support@adguard-dns.io](mailto:support@adguard-dns.io), and they’ll assist you with the right configuration settings.

## How to set up linked IP

The following instructions explain how to connect to the device via **linking IP address**:

1. Open Dashboard.
1. Add a new device or open the settings of a previously connected device.
1. Go to *Use DNS server addresses*.
1. Open *Plain DNS server addresses* and connect the linked IP.

![Linked IP *border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/connect/linked_step4.png)

## DDNS: Why it's useful

Every time a device connects to the network, it gets a new dynamic IP address. When a device disconnects, the DHCP server reassigns IP addresses to the remaining devices. This means dynamic IP addresses can change frequently and unpredictably. Consequently, you'll need to update settings whenever the device is rebooted or the network changes.

To automatically keep the linked IP address updated, you can use DNS. AdGuard DNS will regularly check the IP address of your DDNS domain and link it to your server.

:::note

Dynamic DNS (DDNS) is a service that automatically updates DNS records whenever your IP address changes. It converts network IP addresses into easy-to-read domain names for convenience. The information that connects a name to an IP address is stored in a table on the DNS server. DDNS updates these records whenever there are changes to the IP addresses.

:::

This way, you won’t have to manually update the associated IP address each time it changes.

## DDNS: How to set it up

1. First, you need to check if DDNS is supported by your router settings:

    - To do this, go to *Router settings* → *Network*
    - Locate the DDNS or Dynamic DNS section
    - Navigate to it and verify that the settings are indeed supported

    ![DDNS supported *border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/connect/ddns_step1.png)

1. Register your domain with a popular service like [KeenDNS](https://help.keenetic.com/hc/en-us/articles/360000400919), [DynDNS](https://dyn.com/remote-access/), [NO-IP](https://www.noip.com/), or any other DDNS provider you prefer.
1. Enter the domain in your router settings and sync the configurations.
1. Go to the Linked IP settings to connect the address, then navigate to *Advanced Settings* and click *Configure DDNS*.
1. Input the domain you registered earlier and click *Configure DDNS*.
    ![DDNS supported *border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/connect/ddns_step6.png)

All done, you've successfully set up DDNS!

## Automation of linked IP update via script

### On Windows

The easiest way is to use the Task Scheduler:

1. Create a task:
    - Open the Task Scheduler.
    - Create a new task.
    - Set the trigger to run every 5 minutes.
    - Select *Run Program* as the action.

1. Select a program:
    - In the *Program or Script* field, type `powershell'
    - In the *Add Arguments* field, type:

        `Command "Invoke-WebRequest -Uri 'http://link.adguard.ch/linkip/{ServerID}/{UniqueKey}'"`

1. Save the task.

### On macOS and Linux

On macOS and Linux, the easiest way is to use `cron`:

1. Open crontab:
    - In the terminal, run `crontab -e`.

1. Add a task:
    - Insert the following line:

    `/5 * * * * curl http://link.adguard.ch/linkip/{ServerID}/{UniqueKey}`

    This job will run every 5 minutes.

1. Save crontab.

:::note Important

- Make sure you have `curl` installed on macOS and Linux.
- Remember to copy the address from the settings and replace the `ServerID` and `UniqueKey`.
- If more complex logic or processing of query results is required, consider using scripts (e.g. Bash, Python) in conjunction with a task scheduler or cron.

:::