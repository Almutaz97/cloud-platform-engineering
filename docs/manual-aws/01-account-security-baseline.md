# AWS Account Security Baseline

## Objective

Establish a secure and cost-aware baseline before deploying any AWS infrastructure.

## Configuration

The following controls were verified before beginning the project:

- MFA enabled for the AWS root user
- MFA enabled for the daily IAM user
- Daily administrative access performed through an IAM user instead of the root account
- AWS Budget configured for cost monitoring and alerts

## IAM Setup

The project IAM user currently receives `AdministratorAccess` through an IAM group.

This permission level is intentionally used during the learning and lab phase to avoid permission-related blockers while exploring AWS services.

It is not considered a production least-privilege configuration.

Least-privilege IAM design will be addressed as the project progresses.

## Security Principles

- The root account is reserved for tasks that specifically require root access.
- MFA is required for privileged console access.
- Credentials and private keys must never be committed to the repository.
- Cost monitoring is enabled before provisioning cloud resources.

## Status

Completed.
