Make sure you have provided the following information:

 - [ ] link to your code branch cloned from rhboot/shim-review in the form user/repo@tag
 - [ ] completed README.md file with the necessary information
 - [ ] shim.efi to be signed
 - [ ] public portion of your certificate(s) embedded in shim (the file passed to VENDOR_CERT_FILE)
 - [ ] binaries, for which hashes are added do vendor_db ( if you use vendor_db and have hashes allow-listed )
 - [ ] any extra patches to shim via your own git tree or as files
 - [ ] any extra patches to grub via your own git tree or as files
 - [ ] build logs
 - [ ] a Dockerfile to reproduce the build of the provided shim EFI binaries


###### What organization or people are asking to have this signed:
Microsoft Azure

###### What product or service is this for:
Common Base Linux Delridge (CBL-D)

###### Please create your shim binaries starting with the 15.4 shim release tar file:
###### https://github.com/rhboot/shim/releases/download/15.4/shim-15.4.tar.bz2
###### This matches https://github.com/rhboot/shim/releases/tag/15.4 and contains
###### the appropriate gnu-efi source.
###### Please confirm this as the origin your shim.
Yes, we use the shim 15.4 source

###### What's the justification that this really does need to be signed for the whole world to be able to boot it:
This is for use on Azure VM workloads

###### How do you manage and protect the keys used in your SHIM?
Keys are stored in a hardware HSM with RBAC permission for signing operations (the key itself is never exposed)

###### Do you use EV certificates as embedded certificates in the SHIM?
No, we do not use EV certs at this time.

###### If you use new vendor_db functionality, are any hashes allow-listed, and if yes: for what binaries ?
We don't use vendor_db

###### Is kernel upstream commit 75b0cea7bf307f362057cc778efe89af4c615354 present in your kernel, if you boot chain includes a Linux kernel ?
Yes

###### if SHIM is loading GRUB2 bootloader, are CVEs CVE-2020-14372,
###### CVE-2020-25632, CVE-2020-25647, CVE-2020-27749, CVE-2020-27779,
###### CVE-2021-20225, CVE-2021-20233, CVE-2020-10713, CVE-2020-14308,
###### CVE-2020-14309, CVE-2020-14310, CVE-2020-14311, CVE-2020-15705,
###### ( July 2020 grub2 CVE list + March 2021 grub2 CVE list )
###### and if you are shipping the shim_lock module CVE-2021-3418
###### fixed ?
Yes, the above CVE's are patched, except CVE-2020-15705 and CVE-2021-3418 which do not apply to our implementation.

###### "Please specifically confirm that you add a vendor specific SBAT entry for SBAT header in each binary that supports SBAT metadata
###### ( grub2, fwupd, fwupdate, shim + all child shim binaries )" to shim review doc ?
###### Please provide exact SBAT entries for all SBAT binaries you are booting or planning to boot directly through shim
Yes, we use a vendor-specific SBAT as follows:
```
sbat,1,SBAT Version,sbat,1,https://github.com/rhboot/shim/blob/main/SBAT.md
shim,1,UEFI shim,shim,1,https://github.com/rhboot/shim
shim.cbld,1,CBLD,shim,15.4,https://aka.ms/cbld

sbat,1,SBAT Version,sbat,1,https://github.com/rhboot/shim/blob/main/SBAT.md
grub,1,Free Software Foundation,grub,2.02,https://www.gnu.org/software/grub/
grub.cbld,1,CBLD,grub2,2.02+dfsg1-20+deb10u4,https://aka.ms/cbld
```

##### Were your old SHIM hashes provided to Microsoft ?
We have not previously generated or signed a shim

##### Did you change your certificate strategy, so that affected by CVE-2020-14372, CVE-2020-25632, CVE-2020-25647, CVE-2020-27749,
##### CVE-2020-27779, CVE-2021-20225, CVE-2021-20233, CVE-2020-10713,
##### CVE-2020-14308, CVE-2020-14309, CVE-2020-14310, CVE-2020-14311, CVE-2020-15705 ( July 2020 grub2 CVE list + March 2021 grub2 CVE list )
##### grub2 bootloaders can not be verified ?
We have not previously signed a shim/grub, so we do not have a previous certificate to account for

##### What exact implementation of Secureboot in grub2 ( if this is your bootloader ) you have ?
##### * Upstream grub2 shim_lock verifier or * Downstream RHEL/Fedora/Debian/Canonical like implementation ?
We use the Debian implementation

###### What is the origin and full version number of your bootloader (GRUB or other)?
Using Grub 2.02 plus Debian patches (2.02+dfsg1-20+deb10u4)

###### If your SHIM launches any other components, please provide further details on what is launched
The shim only launches grub

###### If your GRUB2 launches any other binaries that are not Linux kernel in SecureBoot mode,
###### please provide further details on what is launched and how it enforces Secureboot lockdown
Grub only launches the kernel in SecureBoot mode

###### If you are re-using a previously used (CA) certificate, you
###### will need to add the hashes of the previous GRUB2 binaries
###### exposed to the CVEs to vendor_dbx in shim in order to prevent
###### GRUB2 from being able to chainload those older GRUB2 binaries. If
###### you are changing to a new (CA) certificate, this does not
###### apply. Please describe your strategy.
We have not previously used this CA certificate, nor have we
previously signed grub2. If/when we do so in the future, we will add the hashes.

###### How do the launched components prevent execution of unauthenticated code?
Signed grub2 includes common secure boot patches that only load signed binaries.
Signed kernel has a common set of lockdown patches.

###### Does your SHIM load any loaders that support loading unsigned kernels (e.g. GRUB)?
No

###### What kernel are you using? Which patches does it includes to enforce Secure Boot?
5.4.134

###### What changes were made since your SHIM was last signed?
Shim has never been signed

###### What is the SHA256 hash of your final SHIM binary?
`185f66b70a01929676ab62ac1cae4cf1b579ac3d22a20e76baec2817cc2e41b8  shimx64.efi`
