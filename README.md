# Entra Conditional Access Location Policy

Implementation of a location-based Conditional Access policy in Microsoft Entra ID, demonstrating Named Locations, access restriction outside trusted networks, and validation using sign-in log analysis.

## Conditional Access Policy: Location-Based Access Control

## Overview

This lab demonstrates how Microsoft Entra ID Conditional Access can be used to control access based on **network location**. The goal was to understand how trusted locations work, how access can be blocked outside approved networks, and how to safely validate policies using sign-in logs.

This lab was completed in a test tenant and rolled back after validation to avoid user disruption.

## Objectives

* Understand how **Named Locations** are used in Conditional Access
* Configure a **Trusted IP location**
* Create a Conditional Access policy to **block access outside trusted locations**
* Validate enforcement using **Microsoft Entra ID sign-in logs**
* Understand the difference between **Report-only** and **Enforced** policies

## Environment

* Microsoft Entra ID (test tenant)
* Conditional Access
* Test users
* Browser-based sign-ins

## Configuration Summary

### 1. Named Location

* Created a **Trusted Location** using a specific public IP address
* Marked the location as **Trusted**
* Used this location as a condition in Conditional Access policies

### 2. Conditional Access Policy

**Policy name:** `CA-Block-Outside-Trusted-Location`

* **Users:** Specific test users
* **Cloud apps:** All cloud apps
* **Condition:** Network location

  * Include: Any network
  * Exclude: Trusted Location
* **Grant control:** Block access
* **Policy state:** Tested in both *On* and *Report-only* modes

## Testing & Validation

### Test Scenario

* User attempted to sign in from an IP **not included** in the Trusted Location
* Authentication succeeded, but access was **blocked by Conditional Access**

### Validation Method

* Reviewed **Microsoft Entra ID → Sign-in logs**
* Checked the **Conditional Access** tab for the sign-in event
* Confirmed:

  * Policy evaluated
  * Result: **Failure**
  * Grant control: **Block**
  * Reason: Network location condition not met

This confirmed the policy was working as designed.

## Key Learnings

* Conditional Access policies are evaluated **together**
* If **any** policy blocks access, the sign-in fails
* UI error messages are not sufficient for troubleshooting
* **Sign-in logs are the source of truth**
* Report-only mode is essential for safe testing
* Rolling back policies is part of normal security operations

## Next Steps

* Replace block with **Require MFA** for external locations
* Add country-based conditions
* Implement break-glass account exclusions
* Explore sign-in risk and user risk policies
