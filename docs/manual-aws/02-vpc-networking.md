# AWS VPC Networking

## Objective

Build the foundational AWS network manually to understand subnetting,
routing, internet connectivity, and Availability Zone placement.

## Region

Primary AWS Region:

`eu-west-1` — Europe (Ireland)

## VPC

| Resource | Value |
|---|---|
| Name | `cloud-platform-vpc` |
| IPv4 CIDR | `10.0.0.0/16` |
| Tenancy | Default |

The VPC provides the private network boundary for the project.

## Subnet Design

The VPC is divided across two Availability Zones.

| Subnet | Availability Zone | CIDR | Intended Role |
|---|---|---|---|
| `cloud-public-a` | `eu-west-1a` | `10.0.1.0/24` | Public |
| `cloud-public-b` | `eu-west-1b` | `10.0.2.0/24` | Public |
| `cloud-private-a` | `eu-west-1a` | `10.0.11.0/24` | Private |
| `cloud-private-b` | `eu-west-1b` | `10.0.12.0/24` | Private |

A subnet itself is not inherently public or private.

Its routing configuration determines whether it has a direct path to the internet.

## Internet Gateway

An Internet Gateway named `cloud-igw` is attached to the VPC.

The Internet Gateway provides a path between the VPC and the public internet
for resources that have public addressing and appropriate routing.

## Public Route Table

Route table:

`cloud-public-rt`

Routes:

| Destination | Target |
|---|---|
| `10.0.0.0/16` | local |
| `0.0.0.0/0` | `cloud-igw` |

Associated subnets:

- `cloud-public-a`
- `cloud-public-b`

The `0.0.0.0/0` route makes these subnets public from a routing perspective.

## Private Route Table

Route table:

`cloud-private-rt`

Routes:

| Destination | Target |
|---|---|
| `10.0.0.0/16` | local |

Associated subnets:

- `cloud-private-a`
- `cloud-private-b`

The private subnets currently have no route to the public internet.

No NAT Gateway has been configured at this stage.

## Internal VPC Routing

AWS automatically creates the local route:

`10.0.0.0/16 → local`

This allows routing between resources located inside the same VPC.

Actual communication can still be restricted by Security Groups and Network ACLs.

## Current Architecture

```text
                    VPC 10.0.0.0/16

        eu-west-1a                 eu-west-1b

   cloud-public-a             cloud-public-b
    10.0.1.0/24               10.0.2.0/24
          \                       /
           \                     /
             cloud-public-rt
                  |
             0.0.0.0/0
                  |
             cloud-igw
                  |
               Internet


   cloud-private-a            cloud-private-b
    10.0.11.0/24              10.0.12.0/24
          \                       /
           \                     /
             cloud-private-rt
                  |
          10.0.0.0/16 local

```

## Design Decisions

- The VPC was created manually instead of using the default VPC.
- Two Availability Zones are used to prepare the architecture for high availability.
- Public and private workloads are separated into different subnets.
- Public subnets share one public route table because they currently require identical routing behavior.
- Private subnets currently have no internet route.
- NAT Gateway deployment is intentionally postponed until a private workload requires outbound internet access.

## Status

Completed.
