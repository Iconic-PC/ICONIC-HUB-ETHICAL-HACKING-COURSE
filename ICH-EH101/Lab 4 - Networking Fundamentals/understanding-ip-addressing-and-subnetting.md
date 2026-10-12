# ICONIC HUB — NETWORKING FUNDAMENTALS
## Beginner's Guide to IP Addresses, CIDR, Subnet Masks and Subnetting

**Course:** Iconic Hub Ethical Hacking Course  
**Module:** Networking Fundamentals  
**Lesson Level:** Beginner  
**Lesson Title:** Understanding IP Addressing and Subnetting from Scratch

---

## 1. Introduction

Before learning ethical hacking, you need to understand how computers communicate with one another over a network.

One of the most important networking topics is IP addressing. You will often see addresses such as:

- `192.168.1.10`
- `10.0.0.5`
- `172.16.1.20`

You may also see addresses written with a slash, such as:

- `192.168.1.10/24`
- `192.168.1.70/26`

The numbers after the slash can look confusing at first, but they have a specific meaning.

In this lesson, we will learn what IP addresses are, how to identify a network address, what `/24` and `/26` mean, and how to calculate basic subnet information.

## 2. What Is an IP Address?

An IP address is a logical address assigned to a network interface so that it can communicate over an IP network.

Think of an IP address like the address of a house.

A house has an address that helps people locate it. Similarly, an IP address helps network traffic reach the appropriate destination.

For example:

`192.168.1.10`

This is an IPv4 address.

IPv4 addresses contain four numbers separated by dots. Each number is called an octet, and each octet can range from 0 to 255.

Examples of IPv4 addresses include:

- `192.168.1.10`
- `192.168.1.20`
- `10.0.0.5`
- `172.16.1.20`

A device's IP address can change depending on its network configuration.

## 3. Understanding the Difference Between an IP Address and a Network Address

Imagine a street with several houses.

The street represents the network, while the individual houses represent devices connected to that network.

For example:

| Device | IP Address |
|---|---|
| Computer A | `192.168.1.10` |
| Computer B | `192.168.1.20` |
| Phone | `192.168.1.30` |

If these devices use the subnet prefix `/24`, they belong to the same network:

`192.168.1.0/24`

Here is the difference between the terms:

- **IP address:** The address assigned to a network interface.
- **Network address:** The address that identifies the IP network.
- **Subnet mask:** The value used to identify which bits belong to the network portion and which belong to the host portion.
- **CIDR prefix:** The number after the slash that tells us how many bits belong to the network portion.

For example:

IP address: `192.168.1.10/24`

Network address: `192.168.1.0`

The IP address identifies the particular interface, while the network address identifies the network to which it belongs.

## 4. What Is a Subnet Mask?

A subnet mask helps a computer determine which portion of an IP address represents the network and which portion represents the host.

For example:

`255.255.255.0`

This is a subnet mask commonly associated with a `/24` prefix.

When a device has the configuration:

- IP address: `192.168.1.10`
- Subnet mask: `255.255.255.0`

Its network address is:

`192.168.1.0`

The subnet mask helps the device determine whether another destination is on the same local network or whether it needs to send the traffic through a router.

## 5. What Is CIDR?

CIDR stands for **Classless Inter-Domain Routing**.

CIDR notation represents the network prefix using a slash followed by a number.

Examples:

- `/8`
- `/16`
- `/24`
- `/26`

These are called **CIDR prefixes** or **prefix lengths**.

The number after the slash tells us how many of the 32 bits in an IPv4 address belong to the network portion.

For beginners, you can initially think of `/24` as meaning that the first three octets represent the network portion, with the last octet available for addresses within that network.

For example:

`192.168.1.10/24`

The prefix is `/24`, and its subnet mask is:

`255.255.255.0`

However, `/26` divides the address space into smaller networks, so we must calculate the network address differently.

## 6. Is `/24` the Default?

Not always.

Historically, IPv4 used a system called classful addressing.

Under that historical system:

- Class A had a default prefix of `/8`.
- Class B had a default prefix of `/16`.
- Class C had a default prefix of `/24`.

Today, networks use CIDR, so the subnet prefix depends on the actual network configuration.

For example, an IP address such as `192.168.1.10` could be configured with `/24`, `/25`, `/26`, or another valid prefix.

The IP address alone does not tell you which prefix is being used.

Remember these terms:

| Example | What It Is |
|---|---|
| `192.168.1.10` | IP address |
| `/24` | CIDR prefix |
| `255.255.255.0` | Subnet mask |
| `192.168.1.0` | Network address |

These terms are related, but they do not mean the same thing.

## 7. How to Find the Network Address Using `/24`

Let's start with the easiest example.

Given:

- IP address: `192.168.1.70`
- Prefix: `/24`

For a `/24` network, the first three octets represent the network portion.

The IP address is:

`192.168.1.70`

Keep the first three octets:

`192.168.1`

Change the last octet to zero:

`192.168.1.0`

Therefore:

**Network address: `192.168.1.0`**

Here are more examples using `/24`:

| IP Address | CIDR Prefix | Network Address |
|---|---|---|
| `192.168.1.10` | `/24` | `192.168.1.0` |
| `192.168.1.20` | `/24` | `192.168.1.0` |
| `192.168.1.70` | `/24` | `192.168.1.0` |
| `192.168.2.10` | `/24` | `192.168.2.0` |

Notice that the last example belongs to a different network because the third octet is `2` instead of `1`.

**Important:** This simple method applies to `/24`. Other prefixes, such as `/26`, require us to check the subnet boundaries.

## 8. Understanding `/26`

Now let's make the example slightly more difficult.

Given:

- IP address: `192.168.1.70`
- Prefix: `/26`

You might think that the network address is `192.168.1.0`, but that would be incorrect for this `/26` configuration.

A `/26` network contains 64 total IPv4 addresses.

The last octet is divided into four groups:

| Group | Address Range | Network Address |
|---|---|---|
| 1 | 0–63 | `192.168.1.0` |
| 2 | 64–127 | `192.168.1.64` |
| 3 | 128–191 | `192.168.1.128` |
| 4 | 192–255 | `192.168.1.192` |

Our IP address ends in `.70`.

Ask yourself: which range contains 70?

- Is it in the range 0–63? No.
- Is it in the range 64–127? Yes.

Therefore, the network address is:

**`192.168.1.64`**

That is how you find the network address for `192.168.1.70/26`.

## 9. What Is Subnetting?

Subnetting is the process of dividing a larger IP network into smaller networks.

For example, imagine an organization has one network but wants to separate its staff computers, servers, and guest devices.

Subnetting can help divide the available IP address space into smaller networks.

It is useful because it helps organizations:

- Organize devices.
- Manage IP addresses.
- Separate departments and systems.
- Apply different network security rules.
- Reduce unnecessary broadcast traffic.

### Example

Suppose the organization starts with:

`192.168.1.0/24`

It wants to divide this network into four smaller subnets.

Using `/26` gives four groups:

1. `192.168.1.0/26`
2. `192.168.1.64/26`
3. `192.168.1.128/26`
4. `192.168.1.192/26`

Each group contains 64 total addresses.

In an ordinary IPv4 subnet, two addresses are normally reserved: the network address and the broadcast address.

Therefore, each `/26` subnet normally provides 62 usable host addresses.

## 10. Common CIDR Prefixes and Subnet Masks

| CIDR Prefix | Subnet Mask | Total Addresses | Conventional Usable Hosts |
|---|---|---:|---:|
| `/8` | `255.0.0.0` | 16,777,216 | 16,777,214 |
| `/16` | `255.255.0.0` | 65,536 | 65,534 |
| `/24` | `255.255.255.0` | 256 | 254 |
| `/25` | `255.255.255.128` | 128 | 126 |
| `/26` | `255.255.255.192` | 64 | 62 |
| `/27` | `255.255.255.224` | 32 | 30 |
| `/28` | `255.255.255.240` | 16 | 14 |
| `/29` | `255.255.255.248` | 8 | 6 |
| `/30` | `255.255.255.252` | 4 | 2 |

The usable-host figures above apply to conventional IPv4 subnets where the network and broadcast addresses are reserved. Special cases, such as `/31` point-to-point links, are handled differently.

You do not need to memorize this entire table immediately. Begin by understanding `/24` and `/26`.

## 11. Network Address, Broadcast Address, and Usable Host Range

Let's use this example:

`192.168.1.70/26`

We have already calculated that the network address is:

`192.168.1.64`

The complete subnet information is:

- **IP address:** `192.168.1.70`
- **CIDR prefix:** `/26`
- **Subnet mask:** `255.255.255.192`
- **Network address:** `192.168.1.64`
- **First usable host:** `192.168.1.65`
- **Last usable host:** `192.168.1.126`
- **Broadcast address:** `192.168.1.127`
- **Total addresses:** 64
- **Conventional usable hosts:** 62

The network address identifies the subnet. The broadcast address is used to send traffic to all hosts on that IPv4 subnet. The usable host range contains the ordinary device addresses between them.

## 12. Beginner Practice Questions

Try to answer these questions before checking the solutions.

### Question 1

Given:

`192.168.1.50/24`

What is the network address?

### Question 2

Given:

`192.168.1.70/26`

What is the network address?

### Question 3

Given:

`192.168.1.100/26`

Which subnet does the address belong to?

### Question 4

What is `/24` called?

### Question 5

What is the subnet mask for `/24`?

### Question 6

What is the subnet mask for `/26`?

### Question 7

How many total addresses are in a `/26` subnet?

### Question 8

Why can the same IP address be configured with different CIDR prefixes?

---

## 13. Practice Answers

1. `192.168.1.0`
2. `192.168.1.64`
3. `192.168.1.64/26`
4. CIDR prefix or prefix length
5. `255.255.255.0`
6. `255.255.255.192`
7. 64 total addresses
8. Because the prefix is part of the network configuration and determines where the network portion ends and the host portion begins.

---

## 14. Summary

The most important concepts in this lesson are:

- An **IP address** identifies a network interface.
- A **network address** identifies the network to which an address belongs.
- A **subnet mask** determines the network and host portions of an IPv4 address.
- A **CIDR prefix** is the number after the slash, such as `/24` or `/26`.
- `/24` corresponds to `255.255.255.0`.
- `/26` corresponds to `255.255.255.192`.
- A `/24` network contains 256 total addresses.
- A `/26` network contains 64 total addresses.
- Subnetting divides a larger network into smaller networks.
- The first octet alone does not determine a modern network's subnet mask.

### Final Advice for Beginners

Do not rush to memorize every subnet mask or prefix. First, understand what each term means. Then practise finding network addresses using `/24`, followed by `/26`.

Once you understand these two examples, learning the other prefixes becomes much easier.

**Iconic Hub — Secure. Build. Innovate.**
