==================
sysnetwork_selinux
==================

-------------------------------------------
SELinux policy module for system networking
-------------------------------------------

:Author:        Sven Vermeulen <swift@gentoo.org>
:Date:          2014-11-28
:Manual section:        8
:Manual group:          SELinux

DESCRIPTION
===========

The **sysnetwork** SELinux module supports the following core networking
domains: DHCP client and ifconfig.

DHCP Client
-----------

The DHCP client policy works around the *dhcpc_t* domain. It is usually
executed from within an init script, and interacts with the network subsystems
in the Linux kernel in order to obtain an IP address and manage the network
configuration of the system.

Some DHCP clients also have the ability to call additional scripts when an IP
address is obtained (or released), allowing administrators to automate certain
tasks on the system further. Within the SELinux policy, we (Gentoo) try to
handle the hooks through the *dhcp_script_t* domain.

Ifconfig
--------

The *ifconfig* command (and associated *ifconfig_t* domain) is used to manually
set the IP address and other network configurations of the system.

BOOLEANS
========

No booleans are managed through this module.

DOMAINS
=======

dhcpc_t
  The main domain for the DHCP client

dhcpc_script_t
  The domain in which the hooks (pre- and post processing of DHCP operations)
  run

ifconfig_t
  The domain for manual IP address handling (for instance through the
  *ifconfig* or *ip* commands)

POLICY
======

The following interfaces can be used to enhance the default policy with
sysnetwork-related provileges. More details on these interfaces can be found in the
interface HTML documentation, we will not list all available interfaces here.

Domain interaction
------------------

The most interesting definition in the policy is the ``sysnet_dhcpc_script_entry``
interface. It allows for the DHCP script domain (*dhcpc_script_t*) to
execute a particular type (second argument) and transition to a given domain
(first argument).

For instance, to allow a DHCP hook to execute any portage commands::

  sysnet_dhcpc_script_entry(portage_t, portage_exec_t)

It is generally preferred to transition a DHCP hook script as fast as possible
to a specific domain rather than enhancing the *dhcpc_script_t* domain with
additional privileges.

BUGS
====

No specific bugs known.

SEE ALSO
========

* Gentoo and SELinux at https://wiki.gentoo.org/wiki/SELinux
* Gentoo Hardened SELinux Project at
  https://wiki.gentoo.org/wiki/Project:Hardened
