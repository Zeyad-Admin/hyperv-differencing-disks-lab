#  1.2 – Creating and Configuring Differencing Disks (Hyper-V)

# Overview

# 

# This lab demonstrates how to create and manage Hyper-V differencing disks using a generalized parent disk. The objective is to optimize storage usage and deployment time in a testing/lab environment by reusing a single parent VHDX and creating child virtual machines based on it.

# 

# This lab also covers merging a differencing disk to break its dependency on the parent disk.

# 

# Environment Information

# Item	Value

# Host OS	Windows

# Hypervisor	Hyper-V

# Parent VM Name	Parent

# Child VM Name	MGMT

# Parent Disk Name	Parent.vhdx

# Disk Type	VHDX – Dynamic

# Disk Size	200 GB

# Windows Version	Windows Server Datacenter (Trial)

# Task 1: Create Parent VM and Parent Disk

# Step 1: Create Parent Virtual Machine

# 

# Open Hyper-V Manager

# 

# Click New → Virtual Machine

# 

# VM Name: Parent

# 

# Generation: Generation 2

# 

# Startup Memory: 4 GB

# 

# Network: Default Switch



# !\[VM Generation](screenshots/01-parent-vm-generation.png)

# !\[VM Memory](screenshots/02-parent-vm-memory.png)

# !\[VM Summary](screenshots/05-parent-vm-summary.png)

# 



# Step 2: Create Parent Virtual Disk

# 

# Disk Format: VHDX

# 

# Disk Type: Dynamic

# 

# Disk Size: 200 GB

# 

# Disk Name: Parent.vhdx



!\[Parent VHDX Settings](screenshots/03-parent-vhdx-settings.png)

!\[Parent VHDX Location](screenshots/11-parent-vhdx-location.png)



# 

# Step 3: Install Windows Server (Fresh Install)

# 

# Attach Windows Server Datacenter (Trial) ISO

# 

# Start the Parent VM

# 

# Perform a clean installation

# 

# Log in and reach the Windows desktop



!\[Installation Options ISO](screenshots/04-installation-options-iso.png)



# 

# Step 4: Fully Patch the Parent Server

# 

# Open Windows Update

# 

# Install all available updates

# 

# Restart as required

# 

# Verify the system is fully up to date



!\[Windows Update In Progress](screenshots/06-windows-update-in-progress.png)

!\[Windows Up To Date](screenshots/07-windows-up-to-date.png)



# 

# Step 5: Run Sysprep

# What is Sysprep?

# 

# Sysprep (System Preparation Tool) is used to generalize a Windows installation by removing system-specific identifiers such as SIDs. This allows the operating system image to be safely reused for multiple virtual machines.

# 

# Steps:

# 

# Navigate to C:\\Windows\\System32\\Sysprep

# 

# Run sysprep.exe

# 

# Select Out-of-Box Experience (OOBE)

# 

# Check Generalize

# 

# Set Shutdown after completion



!\[Sysprep Configuration](screenshots/08-sysprep-configuration.png)



# 

# Step 6: Delete Parent VM (Keep Disk)

# 

# Right-click the Parent VM

# 

# Select Delete

# 

# Confirm that Parent.vhdx was not deleted



# !\[Delete Parent VM](screenshots/09-delete-parent-vm.png)

# !\[Parent VM Deleted](screenshots/10-parent-vm-deleted-confirmation.png)



# Step 7: Set Parent Disk to Read-Only

# 

# Right-click Parent.vhdx

# 

# Open Properties

# 

# Enable Read-only



!\[Parent Disk Read Only](screenshots/12-parent-disk-readonly.png)



# 

# Task 2: Create Differencing Disk and Child VM

# Step 8: Create Differencing Disk

# 

# Select New → Hard Disk

# 

# Format: VHDX

# 

# Disk Type: Differencing

# 

# Disk Name: MGMT\_Diff.vhdx

# 

# Parent Disk: Parent.vhdx



!\[Differencing Disk Parent Selected](screenshots/13-differencing-disk-parent-selected.png)



# 

# Step 9: Create Child VM Using Differencing Disk

# 

# Create new VM named MGMT

# 

# Generation: Generation 2

# 

# Attach MGMT\_Diff.vhdx

# 

# Start the VM

# 

# Explanation

# 

# Differencing disks store only the changes made to the child virtual machine while the parent disk remains unchanged. This approach significantly reduces storage usage and deployment time. Differencing disks are recommended for testing and lab environments only, not for production use.

# 

# Task 3: Merge Differencing Disk (Break Parent Dependency)



!\[Attach Differencing Disk](screenshots/14-mgmt-attach-differencing-disk.png)

!\[Differencing Disk Attached](screenshots/15-mgmt-diff-disk-attached.png)

!\[MGMT VM Running](screenshots/16-mgmt-vm-running.png)



# Step 10: Shut Down Child VM

# 

# Fully shut down the MGMT virtual machine

# !\[MGMT VM Shutdown](screenshots/17-mgmt-vm-shutdown.png)



# Step 11: Merge the Differencing Disk

# 

# Open Edit Disk

# 

# Select MGMT\_Diff.vhdx

# 

# Choose Merge

# 

# Select Merge to a new disk

# 

# Specify a new VHDX name

# !\[Merge Differencing Disk](screenshots/18-merge-differencing-disk.png)

# !\[Merge Summary](screenshots/19-merge-summary-new-disk.png)



# Step 12: Attach Merged Disk

# 

# Remove the differencing disk from the VM

# 

# Attach the merged VHDX

# 

# Start the MGMT VM

# !\[Merged VHDX Created](screenshots/20-merged-vhdx-created.png)

# !\[Merged Disk Attached](screenshots/21-mgmt-vm-merged-disk-attached.png)



# Final Summary

# 

# In this lab, a parent virtual disk was created and generalized using Sysprep. A child virtual machine was then deployed using a differencing disk to optimize storage usage. Finally, the differencing disk was successfully merged into a standalone VHDX, removing its dependency on the parent disk. This lab demonstrates efficient virtual machine deployment techniques suitable for testing and lab environments.

# 

# Screenshots

# 

# Screenshots for each major step are stored in the /screenshots directory and referenced in this repository.

# 

# Notes

# 

# Differencing disks should never be used in production environments.

# 

# The parent disk must remain unchanged and read-only while differencing disks are in use.

