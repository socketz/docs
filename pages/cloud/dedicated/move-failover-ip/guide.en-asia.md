---
title: Moving a failover IP
excerpt: Find out how to move a failover IP in the Control Panel or via the OVHcloud API
slug: ip-fo-move
section: Network Management
order: 7
---

**Last updated 12th March 2021**

## Objective

Failover IP addresses can be moved between the services you use. This provides an advantage since you can maintain your IP reputation, your SEO and improve the continuity of service of your applications and systems.<br>
With this technology, you can switch IP addresses from one solution to another in less than a minute, with virtually no interruption to services for your users. It is useful for service migrations (e.g. moving projects from development to production), or when switching to a backup server during a technical issue.

**This guide explains how to move a failover IP in your OVHcloud Control Panel or via the OVHcloud API.**

## Requirements

- a [dedicated server](https://www.ovhcloud.com/asia/bare-metal/) in your OVHcloud account
- a [failover IP address](https://www.ovhcloud.com/asia/bare-metal/ip/)
- access to the [OVHcloud Control Panel](https://ca.ovh.com/auth/?action=gotomanager&from=https://www.ovh.com/asia/&ovhSubsidiary=asia)

## Instructions

> [!primary]
> A failover IP cannot be moved from one zone to another. For example, an IP located in the SBG data centre can be moved to GRA or RBX, but cannot be moved to BHS.
>

### Moving an IP from the OVHcloud Control Panel

Log in to the [OVHcloud Control](https://ca.ovh.com/auth/?action=gotomanager&from=https://www.ovh.com/asia/&ovhSubsidiary=asia), click on the `Bare Metal Cloud`{.action} menu, then click on the `IP`{.action} section at the bottom of the column on the left of the page.

![move failover](images/manager01.png){.thumbnail}

The "Service" drop-down menu allows you to filter for Failover IPs.

Click on the `...`{.action} button to the right of the IP address you want to move, then `Move the failover IP`{.action}.

![move failover](images/manager02.png){.thumbnail}

In the pop-up window, select the service to move the IP address to from the menu.

![move failover](images/manager03.png){.thumbnail}

Click `Next`{.action}, then `Confirm`{.action}.

### Moving an IP via the API

Log in to the OVHcloud [API webpage](https://ca.api.ovh.com/).

First, it is best to check if the IP address can be moved.
<br>To check if the IP can be moved to one of your dedicated servers, use the following call:

> [!api]
>
> @api {GET} /dedicated/server/{serviceName}/ipCanBeMovedTo
>

- `serviceName`: the destination dedicated server reference
- `ip`: the failover IP address to move

To move the IP address, use the following call:

> [!api]
>
> @api {POST} /dedicated/server/{serviceName}/ipMove
>

- `serviceName`: the destination dedicated server reference
- `ip`: the failover IP address to move

## Go further

Join our community of users on <https://community.ovh.com/en/>.
