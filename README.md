#This repo aim to contain various selections of tools, configurations, sandboxing tools, firewall, softwares to enhance the privacy and security. I do not claim credits for the work all the work,tools,config,softwares,etc here.
If you wish to collaborate join our matrix room. We aim to provide the tools,configurations file and provide ease of deployment      that you and I need to analyze not only prevent potential threats but render them completely useless agains't  the system.
In this rare case obfuscations with anti-pattern  can help us but wait abusing of anti-patterns can be really bad for readability  and auditing.  The goal is to be able to quickly blendin/obfuscate the system to fools potential watchers/listenn ers . It is far from perfect and to be finished. 
Documentation  ---Make sure you do understand what you are doing this is  not a tutorial aimed at novice but a guideline for people with at least one or more years of immersions in a unix like operating system.

    HardeningWalkthrough

    https://wiki.ubuntu.com/CompilerFlags

    http://people.redhat.com/drepper/nonselsec.pdf

    http://www.suse.de/~krahmer/no-nx.pdf

    http://www.neworder.box.sk/newsread.php?newsid=13007

    http://www.hackinthebox.org/modules.php?op=modload&name=News&file=article&sid=15604&mode=thread&order=0&thold=0

    http://www.phrack.org/archives/issues/58/4.txt

    http://insecure.org/sploits/non-executable.stack.problems.html

    http://www.phrack.org/archives/issues/59/9.txt

    http://www.coresecurity.com/files/attachments/Richarte_Stackguard_2002.pdf

    http://www.redhat.com/archives/fedora-tools-list/2004-September/msg00002.html

    http://www.gentoo.org/proj/en/hardened/hardened-toolchain.xml

    https://fedoraproject.org/wiki/Changes/Harden_All_Packages

    http://labs.mwrinfosecurity.com/notices/security_mechanisms_in_linux_environment__part_1___userspace_memory_protection/

    http://labs.mwrinfosecurity.com/notices/assessing_the_tux_strength_part_2_into_the_kernel/
 IMPORTANT!!!!!!!
I'm not responsible , nor Privy services, Digital Gansgter, DGA, PDG if you do not use the content properly
Make sure that you are aware that it is possible that you break your system
You should always make sure that your system is ready, compatible and that those configs/tools/patch are properly deployed

I do not Own everything in this repository, all credits goes to their original writers
I will start writing several bash scripts/.toml files to automate specific setup maybe at some point I will have enough scripts to consider writing a frontend to execute/manage/remove those.
# DollarLinuxClub is slowly progressing, I did few succesfull build based upon different debian based distro such as devuan, miyoLinux(MakeItYourOwn), mx/AntiX and DemonLinux. I was thinking about making those images availables but they are slightly bloated and unstable. Let me know. 
#9999
9999^ kernel, firmware and headers aim to render any kind of fingerprinting imposible and harden the kernel with various patchset. Do not try to install it inside vmware/virtual box it simply won't work since those relies on identifying the cpu ,# of cores etc, which could potentially build  a

#Dappersec  patchset is a originally a RHEL Based patchset(Fedora) ##Ported to debian it does content better patch and desktop integration ( break less stuff ) than latest patch grsec released and the unofficial one.

**Debian Security Advisories(DSA)**
 https://www.debian.org/doc/manuals/securing-debian-howto/ch7.en.html


	PROCEED AT YOUR OWN RISK !! 




Security, what is security ???? 

# Analyze the Threat model

Always ask yourself questions when you are approaching a system to secure.
Even if those question does look and sound stupid, narrowing it down to it most simplistic form is very important.

By narrowing down your scopes and needs , you will avoid a lot's of mistakes.
    
- [ ] Why do you want to secure your server & services ?
- [ ] How much security do you want or not want?
- [ ] How much devices,apps,softwares,database do you need to secure?
- [ ] How much convenience are you willing to compromise for security, people tend to prefer convenience over security.
- [ ] What are the threats you want to protect against? 
- [ ] What are the specifics to your situation? 
- [ ] Do you think physical access to your server/network a possible attack vector?
- [ ] Do you need to deal with known vulnerables hardwares,softwares,libraries ?
- [ ] How are you hosting it, Is it accessible from public ? 
- [ ] Do you have a way of recovering if your security implementation locks you out of your own server? if you disabled root login or password protected GRUB and deleted your SSH pub key.

# **Linux Files Hierarchy** : 

- [ ] Knowing your system is the  most important part. Security is not a software,services nor a patch
- [ ] It is a constant efforts and in depth knowledge of your system and which services run, which ports are used list goes on....
		

**UEFI Secure Boot*
*
Secure Boot is a feature enabled on most PCs that prevents loading unsigned code, protecting against some kinds of bootkit and rootkit.

Debian can now be installed and run on most PCs with Secure Boot enabled.

It is possible to enable Secure Boot on a system that has an existing Debian installation, if it already boots using UEFI. Before doing this, it's necessary to install shim-signed, grub-efi-amd64-signed or grub-efi-ia32-signed, and a Linux kernel package from buster.

Some features of GRUB and Linux are restricted in Secure Boot mode, to prevent modifications to their code.




Linux kernel and its related files are in /boot directory which is by default as read-write. Changing it to read-only reduces the risk of unauthorized modification of critical boot files. We need to edit /etc/fstab file and insert the line below<

It is important to mount couple partitions with specific mount options and their own partitions
A good example would be the /tmp partition which is often used for privilege escalations 
systems should be separated into different partitions for this will prevent lot's of unwanted executions and manipulations

     /
     /boot
     /usr
     /home
     /tmp
     /var
     /opt
	

* LABEL=/boot     /boot     ext2     defaults,ro     1 2

proc     /proc     proc     defaults,hidepid=2     0     0         # added by unknown on 2019-07-06 @ 06:49:51

***Linux Filesystem Permissions***
 ***Make a backup of fstab and apply secure setting for proc and others***
```
    sudo cp --preserve /etc/fstab /etc/fstab.$(date +"%Y%m%d%H%M%S")
echo -e "\nproc     /proc     proc     defaults,hidepid=2     0     0         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")" | sudo tee -a /etc/fstab

dd if=/dev/zero of=/usr/tmpDISK bs=1024 count=2048000

mkdir /tmpbackup
      cp -Rpf /tmp /tmpbackup
      mount -t tmpfs -o loop,noexec,nosuid,rw /usr/tmpDISK /tmp
      chmod 1777 /tmp
      cp -Rpf /tmpbackup/* /tmp/
      rm -rf /tmpbackup
      echo "/usr/tmpDISK  /tmp    tmpfs   loop,nosuid,nodev,noexec,rw  0 0" >> /etc/fstab
      sudo mount -o remount /tmp
      echo "/dev/sda4   /tmp   tmpfs  loop,nosuid,noexec,rw  0 0 "
       
       ***another example***
          echo "/dev/sda4   /tmp   tmpfs  loop,nosuid,noexec,rw  0 0 "

#Make a backup of OpenSSH server's configuration file /etc/ssh/sshd_config and remove comments to make it easier to read:

    sudo cp --preserve /etc/ssh/sshd_config /etc/ssh/sshd_config.$(date +"%Y%m%d%H%M%S")
    sudo sed -i -r -e '/^#|^$/ d' /etc/ssh/sshd_config
#Change mount point security to nodev, noexec, nosuid (only tested on RHEL)
#/boot
#sed -i "s/\( \/boot.*`grep " \/boot " /etc/fstab | awk '{print $4}'`\)/\1,nodev,noexec,nosuid/" /etc/fstab

#/dev/shm
#sed -i "s/\( \/dev\/shm.*`grep " \/dev\/shm " /etc/fstab | awk '{print $4}'`\)/\1,nodev,noexec,nosuid/" /etc/fstab

#/var
#sed -i "s/\( \/var\/log.*`grep " \/var " /etc/fstab | awk '{print $4}'`\)/\1,nodev,noexec,nosuid/" /etc/fstab

#/var/log
#sed -i "s/\( \/var\/log.*`grep " \/var\/log " /etc/fstab | awk '{print $4}'`\)/\1,nodev,noexec,nosuid/" /etc/fstab

#/tmp
#sed -i "s/\( \/tmp.*`grep " \/tmp " /etc/fstab | awk '{print $4}'`\)/\1,nodev,noexec,nosuid/" /etc/fstab

#/home
#sed -i "s/\( \/home.*`grep " \/home " /etc/fstab | awk '{print $4}'`\)/\1,nodev,nosuid/" /etc/fstab
```
**Optional hardening of APT**

#All methods provided by APT (e.g. http, and https) except for cdrom, gpgv, and rsh can make use of seccomp-BPF sandboxing as #supplied by the Linux kernel to restrict the list of allowed system calls, and trap all others with a SIGSYS signal. This #sandboxing is currently opt-in and needs to be enabled with:

      APT::Sandbox::Seccomp is a boolean to turn it on/off
    

**Two options can be used to configure this further:**

      APT::Sandbox::Seccomp::Trap is a list of names of more syscalls to trap
      APT::Sandbox::Seccomp::Allow is a list of names of more syscalls to allow
    


# Make sure sensitives files are owned by root and with the rights permissions

```

chmod -f 0700 /etc/cron.monthly/*
chmod -f 0700 /etc/cron.weekly/*
chmod -f 0700 /etc/cron.daily/*
chmod -f 0700 /etc/cron.hourly/*
chmod -f 0700 /etc/cron.d/*
chmod -f 0400 /etc/cron.allow
chmod -f 0400 /etc/cron.deny
chmod -f 0400 /etc/crontab
chmod -f 0400 /etc/at.allow
chmod -f 0400 /etc/at.deny
chmod -f 0700 /etc/cron.daily
chmod -f 0700 /etc/cron.weekly
chmod -f 0700 /etc/cron.monthly
chmod -f 0700 /etc/cron.hourly
chmod -f 0700 /var/spool/cron
chmod -f 0600 /var/spool/cron/*
chmod -f 0700 /var/spool/at
chmod -f 0600 /var/spool/at/*
chmod -f 0400 /etc/anacrontab


#File permissions and ownerships
chmod -f 1777 /tmp
chown -f root:root /var/crash
chown -f root:root /var/cache/mod_proxy
chown -f root:root /var/lib/dav
chown -f root:root /usr/bin/lockfile
chown -f rpcuser:rpcuser /var/lib/nfs/statd
chown -f adm:adm /var/adm
chmod -f 0600 /var/crash
chown -f root:root /bin/mail
chmod -f 0700 /sbin/reboot
chmod -f 0700 /sbin/shutdown
chmod -f 0600 /etc/ssh/ssh*config
chown -f root:root /root
chmod -f 0700 /root
chmod -f 0500 /usr/bin/ypcat
chmod -f 0700 /usr/sbin/usernetctl
chmod -f 0700 /usr/bin/rlogin
chmod -f 0700 /usr/bin/rcp
chmod -f 0640 /etc/pam.d/system-auth*
chmod -f 0640 /etc/login.defs
chmod -f 0750 /etc/security
chmod -f 0600 /etc/audit/audit.rules
chown -f root:root /etc/audit/audit.rules
chmod -f 0600 /etc/audit/auditd.conf
chown -f root:root /etc/audit/auditd.conf
chmod -f 0600 /etc/auditd.conf
chmod -f 0744 /etc/rc.d/init.d/auditd
chown -f root /sbin/auditctl
chmod -f 0750 /sbin/auditctl
chown -f root /sbin/auditd
chmod -f 0750 /sbin/auditd
chmod -f 0750 /sbin/ausearch
chown -f root /sbin/ausearch
chown -f root /sbin/aureport
chmod -f 0750 /sbin/aureport
chown -f root /sbin/autrace
chmod -f 0750 /sbin/autrace
chown -f root /sbin/audispd
chmod -f 0750 /sbin/audispd
chmod -f 0444 /etc/bashrc
chmod -f 0444 /etc/csh.cshrc
chmod -f 0444 /etc/csh.login
chmod -f 0600 /etc/cups/client.conf
chmod -f 0600 /etc/cups/cupsd.conf
chown -f root:sys /etc/cups/client.conf
chown -f root:sys /etc/cups/cupsd.conf
chmod -f 0600 /etc/grub.conf
chown -f root:root /etc/grub.conf
chmod -f 0600 /boot/grub2/grub.cfg
chown -f root:root /boot/grub2/grub.cfg
chmod -f 0600 /boot/grub/grub.cfg
chown -f root:root /boot/grub/grub.cfg
chmod -f 0444 /etc/hosts
chown -f root:root /etc/hosts
chmod -f 0600 /etc/inittab
chown -f root:root /etc/inittab
chmod -f 0444 /etc/mail/sendmail.cf
chown -f root:bin /etc/mail/sendmail.cf
chmod -f 0600 /etc/ntp.conf
chmod -f 0640 /etc/security/access.conf
chmod -f 0600 /etc/security/console.perms
chmod -f 0600 /etc/security/console.perms.d/50-default.perms
chmod -f 0600 /etc/security/limits
chmod -f 0444 /etc/services
chmod -f 0444 /etc/shells
chmod -f 0644 /etc/skel/.*
chmod -f 0600 /etc/skel/.bashrc
chmod -f 0600 /etc/skel/.bash_profile
chmod -f 0600 /etc/skel/.bash_logout
chmod -f 0440 /etc/sudoers
chown -f root:root /etc/sudoers
chmod -f 0600 /etc/sysctl.conf
chown -f root:root /etc/sysctl.conf
chown -f root:root /etc/sysctl.d/*
chmod -f 0700 /etc/sysctl.d
chmod -f 0600 /etc/sysctl.d/*
chmod -f 0600 /etc/syslog.conf
chmod -f 0600 /var/yp/binding
chown -f root:$AUDIT /var/log
chown -Rf root:$AUDIT /var/log/*
chmod -Rf 0640 /var/log/*
chmod -Rf 0640 /var/log/audit/*
chmod -f 0755 /var/log
chmod -f 0750 /var/log/syslog /var/log/audit
chmod -f 0600 /var/log/lastlog*
chmod -f 0600 /var/log/cron*
chmod -f 0600 /var/log/btmp
chmod -f 0660 /var/log/wtmp
chmod -f 0444 /etc/profile
chmod -f 0700 /etc/rc.d/rc.local
chmod -f 0400 /etc/securetty
chmod -f 0700 /etc/rc.local
chmod -f 0750 /usr/bin/wall
chown -f root:tty /usr/bin/wall
chown -f root:users /mnt
chown -f root:users /media
chmod -f 0644 /etc/.login
chmod -f 0644 /etc/profile.d/*
chown -f root /etc/security/environ
chown -f root /etc/xinetd.d
chown -f root /etc/xinetd.d/*
chmod -f 0750 /etc/xinetd.d
chmod -f 0640 /etc/xinetd.d/*
chmod -f 0640 /etc/selinux/config
chmod -f 0750 /usr/bin/chfn
chmod -f 0750 /usr/bin/chsh
chmod -f 0750 /usr/bin/write
chmod -f 0750 /sbin/mount.nfs
chmod -f 0750 /sbin/mount.nfs4
chmod -f 0700 /usr/bin/ldd #0400 FOR SOME SYSTEMS
chmod -f 0700 /bin/traceroute
chown -f root:root /bin/traceroute
chmod -f 0700 /usr/bin/traceroute6*
chown -f root:root /usr/bin/traceroute6
chmod -f 0700 /bin/tcptraceroute
chmod -f 0700 /sbin/iptunnel
chmod -f 0700 /usr/bin/tracpath*
chmod -f 0644 /dev/audio
chown -f root:root /dev/audio
chmod -f 0644 /etc/environment
chown -f root:root /etc/environment
chmod -f 0600 /etc/modprobe.conf
chown -f root:root /etc/modprobe.conf
chown -f root:root /etc/modprobe.d
chown -f root:root /etc/modprobe.d/*
chmod -f 0700 /etc/modprobe.d
chmod -f 0600 /etc/modprobe.d/*
chmod -f o-w /selinux/*
#umask 077 /etc/*
chmod -f 0755 /etc
chmod -f 0644 /usr/share/man/man1/*
chmod -Rf 0644 /usr/share/man/man5
chmod -Rf 0644 /usr/share/man/man1
chmod -f 0600 /etc/yum.repos.d/*
chmod -f 0640 /etc/fstab
chmod -f 0755 /var/cache/man
chmod -f 0755 /etc/init.d/atd
chmod -f 0750 /etc/ppp/peers
chmod -f 0755 /bin/ntfs-3g
chmod -f 0750 /usr/sbin/pppd
chmod -f 0750 /etc/chatscripts
chmod -f 0750 /usr/local/share/ca-certificates

#DISA STIG file ownsership
chmod -f 0755 /bin/csh
chmod -f 0755 /bin/jsh
chmod -f 0755 /bin/ksh
chmod -f 0755 /bin/rsh
chmod -f 0755 /bin/sh
chmod -f 0640 /dev/kmem
chown -f root:sys /dev/kmem
chmod -f 0640 /dev/mem
chown -f root:sys /dev/mem
chmod -f 0666 /dev/null
chown -f root:sys /dev/null
chmod -f 0755 /etc/csh
chmod -f 0755 /etc/jsh
chmod -f 0755 /etc/ksh
chmod -f 0755 /etc/rsh
chmod -f 0755 /etc/sh
chmod -f 0644 /etc/aliases
chown -f root:root /etc/aliases
chmod -f 0640 /etc/exports
chown -f root:root /etc/exports
chmod -f 0640 /etc/ftpusers
chown -f root:root /etc/ftpusers
chmod -f 0664 /etc/host.lpd
chmod -f 0440 /etc/inetd.conf
chown -f root:root /etc/inetd.conf
chmod -f 0644 /etc/mail/aliases
chown -f root:root /etc/mail/aliases
chmod -f 0644 /etc/passwd
chown -f root:root /etc/passwd
chmod -f 0400 /etc/shadow
chown -f root:root /etc/shadow
chmod -f 0600 /etc/uucp/L.cmds
chown -f uucp:uucp /etc/uucp/L.cmds
chmod -f 0600 /etc/uucp/L.sys
chown -f uucp:uucp /etc/uucp/L.sys
chmod -f 0600 /etc/uucp/Permissions
chown -f uucp:uucp /etc/uucp/Permissions
chmod -f 0600 /etc/uucp/remote.unknown
chown -f root:root /etc/uucp/remote.unknown
chmod -f 0600 /etc/uucp/remote.systems
chmod -f 0600 /etc/uccp/Systems
chown -f uucp:uucp /etc/uccp/Systems
chmod -f 0755 /sbin/csh
chmod -f 0755 /sbin/jsh
chmod -f 0755 /sbin/ksh
chmod -f 0755 /sbin/rsh
chmod -f 0755 /sbin/sh
chmod -f 0755 /usr/bin/csh
chmod -f 0755 /usr/bin/jsh
chmod -f 0755 /usr/bin/ksh
chmod -f 0755 /usr/bin/rsh
chmod -f 0755 /usr/bin/sh
chmod -f 1777 /var/mail
chmod -f 1777 /var/spool/uucppublic
#or thisset of perm is possible adjust it for your need
#Set all files in ``.ssh`` to ``600``
chmod 700 ~/.ssh && chmod 600 ~/.ssh/*
chmod o= /etc/ftpusers 
chmod o= /etc/group 
chmod o= /etc/hosts
chmod o= /etc/hosts.allow 
chmod o= /etc/hosts.equiv
chmod o= /etc/hosts.lpd 
chmod o= /etc/inetd.conf
chmod o= /etc/login.access 
chmod o= /etc/login.conf 
chmod o= /etc/newsyslog.conf
chmod o= /etc/rc.conf 
chmod o= /etc/ssh/sshd_config 
chmod o= /etc/sysctl.conf
chmod o= /etc/syslog.conf 
chmod o= /etc/ttys 
chmod o= /etc/fstab
chown root:root /etc/anacrontab
chmod og-rwx /etc/anacrontab
chown root:root /etc/crontab
chmod og-rwx /etc/crontab
chown root:root /etc/cron.hourly
chmod og-rwx /etc/cron.hourly
chown root:root /etc/cron.daily
chmod og-rwx /etc/cron.daily
chown root:root /etc/cron.weekly
chmod og-rwx /etc/cron.weekly
chown root:root /etc/cron.monthly
chmod og-rwx /etc/cron.monthly
chown root:root /etc/cron.d
chmod og-rwx /etc/cron.d
chown root:root /etc/ssh/sshd_config
chmod 600 /etc/ssh/sshd_config
chown root:root /etc/grub.conf
chown root:root /etc/fstab
chmod og-rwx /etc/grub.conf
chmod 710 /root "or" chmod 700 /root
chmod o= /var/log 
chmod 644 /etc/passwd
chown root:root /etc/passwd
chmod 644 /etc/group
chown root:root /etc/group
chmod 600 /etc/shadow
chown root:root /etc/shadow
chmod 600 /etc/gshadow
chown root:root /etc/gshadow		
chmod 700 /var/log/audit
chmod 740 /etc/rc.d/init.d/iptables
chmod 740 /sbin/iptables
chmod 600 /etc/rsyslog.conf
chmod 640 /etc/security/access.conf
chmod 600 /etc/sysctl.conf
chown root:root /etc/anacrontab
chmod og-rwx /etc/anacrontab
chown root:root /etc/crontab
chmod og-rwx /etc/crontab
chown root:root /etc/cron.hourly
chmod og-rwx /etc/cron.hourly
chown root:root /etc/cron.daily
chmod og-rwx /etc/cron.daily
chown root:root /etc/cron.weekly
chmod og-rwx /etc/cron.weekly
chown root:root /etc/cron.monthly
chmod og-rwx /etc/cron.monthly
chown root:root /etc/cron.d
chmod og-rwx /etc/cron.d
chown root:root /boot/grub/grub.cfg
chmod og-rwx /boot/grub/grub.cfg
chown root:root /boot/grub/grub.cfg
chmod og-rwx /boot/grub/grub.cfg


chown root:root /etc/cron*
chmod og-rwx /etc/cron*
#Ensure at/cron is restricted to authorized users 

touch /etc/cron.allow
touch /etc/at.allow

chmod og-rwx /etc/cron.allow /etc/at.allow
chown root:root /etc/cron.allow /etc/at.allow
chmod -R g-wx,o-rwx /var/log/*



chown root:root /etc/cron
```
```
#ClamAV permissions and ownership
if [[ -d /usr/local/share/clamav ]]; then
  passwd -l clamav 2>/dev/null
  usermod -s /sbin/nologin clamav 2>/dev/null
  chmod -f 0755 /usr/local/share/clamav
  chown -f root:clamav /usr/local/share/clamav
  chown -f root:clamav /usr/local/share/clamav/*.cvd
  chmod -f 0664 /usr/local/share/clamav/*.cvd
  mkdir -p /var/log/clamav
  chown -f root:$AUDIT /var/log/clamav
  chmod -f 0640 /var/log/clamav
fi
if [[ -d /var/clamav ]]; then
  passwd -l clamav 2>/dev/null
  usermod -s /sbin/nologin clamav 2>/dev/null
  chmod -f 0755 /var/clamav
  chown -f root:clamav /var/clamav
  chown -f root:clamav /var/clamav/*.cvd
  chmod -f 0664 /var/clamav/*.cvd
  mkdir -p /var/log/clamav
  chown -f root:$AUDIT /var/log/clamav
  chmod -f 0640 /var/log/clamav
fi
```


```
#Disable ctrl-alt-delete RHEL 6+
if [[ -f /etc/init/control-alt-delete.conf ]]; then
  if [[ `grep ^exec /etc/init/control-alt-delete.conf` != "" ]]; then
    sed -i 's/^exec/#exec/g' /etc/init/control-alt-delete.conf
  fi
fi
```
```
#Disable ctrl-alt-delete RHEL 5+
if [[ -f /etc/inittab ]]; then
  if [[ `grep ^ca:: /etc/inittab` != "" ]]; then
    sed -i 's/^ca::/#ca::/g' /etc/inittab
  fi
fi
```
```
#if u got rpm 
#Remove security related packages
if [[ -f /bin/rpm ]]; then
  rpm -ev nc 2>/dev/null
  rpm -ev vsftpd 2>/dev/null
  rpm -ev nmap 2>/dev/null
  rpm -ev telnet-server 2>/dev/null
  rpm -ev rdate 2>/dev/null
  rpm -ev tcpdump 2>/dev/null
  rpm -ev vnc-server 2>/dev/null
  rpm -ev tigervnc-server 2>/dev/null
  rpm -ev wireshark 2>/dev/null
  rpm -ev --allmatches --nodeps wireless-tools 2>/dev/null


#remove unwanted stuff***CAN REMOVE IMPORTANT STUFF DUE TO DEPENDENCIES***
  apt-get autoremove -y vsftpd 2>/dev/null
  apt-get autoremove -y nmap 2>/dev/null
  apt-get autoremove -y telnetd 2>/dev/null
  apt-get autoremove -y rdate 2>/dev/null
  apt-get autoremove -y tcpdump 2>/dev/null
  apt-get autoremove -y vnc4server 2>/dev/null
  apt-get autoremove -y vino 2>/dev/null
  apt-get autoremove -y wireshark 2>/dev/null
  apt-get autoremove -y bind9-host 2>/dev/null
  apt-get autoremove -y libbind9-90 2>/dev/null
```
```
#Account management and cleanup

  userdel -f games 2>/dev/null
  userdel -f news 2>/dev/null
  userdel -f gopher 2>/dev/null
  userdel -f tcpdump 2>/dev/null
  userdel -f shutdown 2>/dev/null
  userdel -f halt 2>/dev/null
  userdel -f sync 2>/dev/null
  userdel -f ftp 2>/dev/null
  userdel -f operator 2>/dev/null
  userdel -f lp 2>/dev/null
  userdel -f uucp 2>/dev/null
  userdel -f irc 2>/dev/null
  userdel -f gnats 2>/dev/null
  userdel -f pcap 2>/dev/null
  userdel -f netdump 2>/dev/null
```
```
Only root account have UID 0 with full permissions to access the system. Type the following command to display all accounts with UID set to 0:
# awk -F: '($3 == "0") {print}' /etc/passwd

df --local -P | awk {'if (NR!=1) print $6'} | xargs -I '{}' find '{}' -xdev -type d -perm -0002 2>/dev/null | xargs chmod a+t

```
```
First, Restrict Core Dumps by:

    Adding hard core 0 to the “/etc/security/limits.conf” file
    Adding fs.suid_dumpable = 0 to the “/etc/sysctl.conf” file

Second, configure Exec Shield by:

    Adding kernel.exec-shield = 1 to the “/etc/sysctl.conf” file

Third, enable randomized Virtual Memory Region Placement by:

    Adding kernel.randomize_va_space = 2 to the “/etc/sysctl.conf” file


```
```
#GDM user RHEL 5 is unlocked out-of-the-box
passwd -l gdm 2>/dev/null


#Set password settings for all accounts in shadow
#sed -i 's/0:99999:7/'"$PASS_CHANG:$PASS_EXP:$PASS_WARN"'/' /etc/shadow
```
```
#See all set user id files:
find / -perm +4000
# See all group id files
find / -perm +2000
# Or combine both in a single command
find / \( -perm -4000 -o -perm -2000 \) -print
find / -path -prune -o -type f -perm +6000 -ls
```
```
find /dir -xdev -type d \( -perm -0002 -a ! -perm -1000 \) -print
 echo 'install usb-storage /bin/true' * *  /etc/modprobe.d/disable-usb-storage.conf
# echo "blacklist firewire-core" * *  /etc/modprobe.d/firewire.conf
# echo "blacklist thunderbolt" * *  /etc/modprobe.d/thunderbolt.conf
sudo dpkg-statoverride --update --add root sudo 4750 /bin/su
		
find / -xdev \( -perm -4000 -o -perm -2000 \) -type f | awk '{print \
"-a always,exit -F path=" $1 " -F perm=x -F auid>=1000 -F auid!=4294967295 \
-k privileged" } ' >> /etc/audit/audit.rules
```
```
echo " " >> /etc/audit/audit.rules
echo "#End of Audit Rules" >> /etc/audit/audit.rules
echo "-e 2" >>/etc/audit/audit.rules

cp /etc/audit/audit.rules /etc/audit/rules.d/audit.rules

*```

sudo /usr/sbin/logwatch --output stdout --format text --range yesterday --service all
sudo cp --preserve /etc/cron.daily/00logwatch /etc/cron.daily/00logwatch.$(date +"%Y%m%d%H%M%S")
sudo chmod -x /etc/cron.daily/00logwatch.*
sudo sed -i -r -e "s,^($(sudo which logwatch).*?),# \1         # commented by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")\n$(sudo which logwatch) --output mail --format html --mailto root --range yesterday --service all         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")," /etc/cron.daily/00logwatch
sudo apt install apt-transport-https ca-certificates host
sudo wget -O - https://packages.cisofy.com/keys/cisofy-software-public.key | sudo apt-key add -
sudo echo "deb https://packages.cisofy.com/community/lynis/deb/ stable main" | sudo tee /etc/apt/sources.list.d/cisofy-lynis.list
sudo apt update
sudo apt install lynis host
cat << EOF | sudo tee /etc/rsyslog.d/10-iptables.conf
:msg, contains, "[IPTABLES] " /var/log/iptables.log
& stop
EOF

sudo sed -i -r -e "s/^(IPT_SYSLOG_FILE\s+)([^;]+)(;)$/# \1\2\3       # commented by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")\n\1\/var\/log\/iptables.log\3       # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")/" /etc/psad/psad.conf
sudo psad -R
sudo psad --sig-update
sudo psad -H
sudo service rsyslog restart

sudo cp --preserve /etc/profile /etc/profile.$(date +"%Y%m%d%H%M%S")
sudo cp --preserve /etc/bash.bashrc /etc/bash.bashrc.$(date +"%Y%m%d%H%M%S")
sudo cp --preserve /etc/login.defs /etc/login.defs.$(date +"%Y%m%d%H%M%S")
sudo cp --preserve /root/.bashrc /root/.bashrc.$(date +"%Y%m%d%H%M%S")
echo -e "\numask 0027         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")" | sudo tee -a /etc/profile /etc/bash.bashrc
echo -e "\nUMASK 0027         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")" | sudo tee -a /etc/login.defs
echo -e "\numask 0077         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")" | sudo tee -a /root/.bashrc

```
```
#echo "* hard core 0" >> /etc/security/limits.conf
cp templates/sysctl-CIS.conf /etc/sysctl.conf
sysctl -e -p
Afters system, files and other permissions we will edit out kernel setting in `/etc/sysctl.conf`
```
````
fs.file-max = 65535 		
fs.protected_hardlinks = 1 		
fs.protected_symlinks = 1 		
fs.suid_dumpable = 0 		
kernel.core_uses_pid = 1 		
kernel.ctrl-alt-del = 0 		
kernel.kptr_restrict = 2 		
kernel.maps_protect = 1 		
kernel.msgmax = 65535 		
kernel.msgmnb = 65535 		
kernel.pid_max = 65535 	
kernel.sysrq = 0
kernel.core_uses_pid = 1
kernel.dmesg_restrict = 1
kernel.panic = 60
kernel.panic_on_oops = 60
kernel.perf_event_paranoid = 2
kernel.randomize_va_space = 2
kernel.yama.ptrace_scope = 2
net.ipv4.tcp_congestion_control=htcp
kernel.maps_protect = 1
kernel.ctrl-alt-del = 0
fs.file-max = 100000
net.core.netdev_max_backlog = 100000
net.core.netdev_budget = 50000
net.core.netdev_budget_usecs = 5000
net.core.somaxconn = 1024
net.core.rmem_default = 1048576
net.core.rmem_max = 16777216
net.core.wmem_default = 1048576
net.core.wmem_max = 16777216
net.core.optmem_max = 65536
net.ipv4.tcp_rmem = 4096 1048576 2097152
net.ipv4.tcp_wmem = 4096 65536 16777216
net.ipv4.udp_rmem_min = 8192
net.ipv4.udp_wmem_min = 8192
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_max_tw_buckets = 2000000
net.ipv4.tcp_fin_timeout = 10
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_keepalive_time = 60
net.ipv4.tcp_keepalive_intvl = 10
net.ipv4.tcp_keepalive_probes = 6
net.ipv4.tcp_mtu_probing = 1
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_rfc1337 = 1
net.ipv4.tcp_fin_timeout = 10
vm.overcommit_ratio = 50
vm.overcommit_memory = 0
vm.mmap_min_addr = 4096
vm.min_free_kbytes = 65535
net.unix.max_dgram_qlen = 50
net.ipv4.conf.default.log_martians = 1
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.secure_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0
net.ipv4.icmp_echo_ignore_all = 1
vm.dirty_background_bytes = 4194304
vm.dirty_bytes = 4194304
kernel.kptr_restrict = 2
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.ipv4.tcp_challenge_ack_limit = 1000000
net.ipv4.tcp_invalid_ratelimit = 500
net.ipv4.tcp_synack_retries = 2
net.ipv6.conf.all.accept_ra = 0
net.ipv6.conf.all.accept_redirects = 0
fs.file-max = 65535
#Allow for more PIDs 
kernel.pid_max = 65536
#Increase system IP port limits
net.ipv4.ip_local_port_range = 2000 65000
net.ipv4.tcp_wmem = 8192 65536 16777216
net.ipv4.udp_rmem_min = 16384
net.ipv4.udp_wmem_min = 16384
net.ipv4.tcp_tw_recycle = 0
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_sack = 0
net.ipv4.tcp_reordering = 3
net.ipv4.tcp_no_metrics_save = 1
net.ipv4.tcp_moderate_rcvbuf = 1
net.ipv4.tcp_orphan_retries = 0
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.conf.lo.rp_filter = 1
net.ipv4.conf.lo.log_martians = 0
net.ipv4.conf.eth0.rp_filter = 1
net.ipv4.conf.eth0.log_martians = 0

net.ipv6.conf.eth0.accept_ra_rtr_pref = 0
net.netfilter.nf_conntrack_max = 2000000
net.netfilter.nf_conntrack_tcp_loose = 0

# increase system file descriptor limit    
fs.file-max = 65535
#Increase system IP port limits
net.ipv4.ip_local_port_range = 2000 65000

net.ipv4.tcp_wmem = 8192 65536 16777216
net.ipv4.udp_rmem_min = 16384
net.ipv4.udp_wmem_min = 16384
net.ipv4.tcp_tw_recycle = 0
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_reordering = 3
net.ipv4.tcp_no_metrics_save = 1
net.ipv4.tcp_moderate_rcvbuf = 1
net.ipv4.tcp_orphan_retries = 0
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.conf.lo.rp_filter = 1
net.ipv4.conf.lo.log_martians = 0
net.ipv4.conf.eth0.rp_filter = 1
net.ipv4.conf.eth0.log_martians = 0
net.core.bpf_jit_harden=2
kernel.dmesg_restrict=1
kernel.kptr_restrict=1
kernel.kexec_load_disabled = 1
kernel.unprivileged_bpf_disabled=1
net.ipv4.ip_default_ttl = 255
net.ipv4.icmp_echo_ignore_all = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1

kernel.randomize_va_space = 2 		
kernel.shmall = 268435456 		
kernel.shmmax = 268435456 		
kernel.sysrq = 0 		
net.core.default_qdisc = fq 		
net.core.dev_weight = 64 		
net.core.netdev_max_backlog = 16384 		
net.core.optmem_max = 65535 		
net.core.rmem_default = 262144 		
net.core.rmem_max = 16777216 		
net.core.somaxconn = 32768 		
net.core.wmem_default = 262144 		
net.core.wmem_max = 16777216 		
net.ipv4.conf.all.accept_redirects = 0 		
net.ipv4.conf.all.accept_source_route = 0 		
net.ipv4.conf.all.bootp_relay = 0 		
net.ipv4.conf.all.forwarding = 0 		
net.ipv4.conf.all.log_martians = 1 		
net.ipv4.conf.all.proxy_arp = 0 		
net.ipv4.conf.all.rp_filter = 1 		
net.ipv4.conf.all.secure_redirects = 0 		
net.ipv4.conf.all.send_redirects = 0 		
net.ipv4.conf.default.accept_redirects = 0 		
net.ipv4.conf.default.accept_source_route = 0 		
net.ipv4.conf.default.forwarding = 0 		
net.ipv4.conf.default.log_martians = 1 		
net.ipv4.conf.default.rp_filter = 1 		
net.ipv4.conf.default.secure_redirects = 0 		
net.ipv4.conf.default.send_redirects = 0 			
net.ipv4.conf.lo.accept_redirects = 0 		
net.ipv4.conf.lo.accept_source_route = 0 		
net.ipv4.conf.lo.log_martians = 0 		
net.ipv4.conf.lo.rp_filter = 1 		
net.ipv4.icmp_echo_ignore_all = 1 		
net.ipv4.icmp_echo_ignore_broadcasts = 1 		
net.ipv4.icmp_ignore_bogus_error_responses = 1 			
net.ipv4.ip_local_port_range = 2000 65000 		
net.ipv4.ipfrag_high_thresh = 262144 		
net.ipv4.ipfrag_low_thresh = 196608 		
net.ipv4.neigh.default.gc_interval = 30 		
net.ipv4.neigh.default.gc_thresh1 = 32 		
net.ipv4.neigh.default.gc_thresh2 = 1024 		
net.ipv4.neigh.default.gc_thresh3 = 2048 		
net.ipv4.neigh.default.proxy_qlen = 96 		
net.ipv4.neigh.default.unres_qlen = 6 		
net.ipv4.route.flush = 1 			
net.ipv4.tcp_fastopen = 3 		
net.ipv4.tcp_fin_timeout = 15 		
net.ipv4.tcp_keepalive_intvl = 15 		
net.ipv4.tcp_keepalive_probes = 5 		
net.ipv4.tcp_keepalive_time = 1800 		
net.ipv4.tcp_max_orphans = 16384 		
net.ipv4.tcp_max_syn_backlog = 2048 		
net.ipv4.tcp_max_tw_buckets = 1440000 		
net.ipv4.tcp_moderate_rcvbuf = 1 		
net.ipv4.tcp_no_metrics_save = 1 		
net.ipv4.tcp_orphan_retries = 0 		
net.ipv4.tcp_reordering = 3 		
net.ipv4.tcp_retries1 = 3 		
net.ipv4.tcp_retries2 = 15 		
net.ipv4.tcp_rfc1337 = 1 		
net.ipv4.tcp_rmem = 8192 87380 16777216 				
net.ipv4.tcp_slow_start_after_idle = 0 		
net.ipv4.tcp_syn_retries = 5 		
net.ipv4.tcp_synack_retries = 2 		
net.ipv4.tcp_syncookies = 1 		
net.ipv4.tcp_timestamps = 1 		
net.ipv4.tcp_tw_recycle = 0 		
net.ipv4.tcp_tw_reuse = 1 		
net.ipv4.tcp_window_scaling = 0 		
net.ipv4.tcp_wmem = 8192 65536 16777216 		
net.ipv4.udp_rmem_min = 16384 		
net.ipv4.udp_wmem_min = 16384 		
net.ipv6.conf.all.accept_ra=0 		
net.ipv6.conf.all.accept_redirects = 0 		
net.ipv6.conf.all.accept_source_route = 0 		
net.ipv6.conf.all.autoconf = 0 		
net.ipv6.conf.all.forwarding = 0 		
net.ipv6.conf.default.accept_ra_defrtr = 0 		
net.ipv6.conf.default.accept_ra_pinfo = 0 		
net.ipv6.conf.default.accept_ra_rtr_pref = 0 		
net.ipv6.conf.default.accept_ra=0 		
net.ipv6.conf.default.accept_redirects = 0 		
net.ipv6.conf.default.accept_source_route = 0 		
net.ipv6.conf.default.autoconf = 0 		
net.ipv6.conf.default.dad_transmits = 0 		
net.ipv6.conf.default.forwarding = 0 		
net.ipv6.conf.default.max_addresses = 1 		
net.ipv6.conf.default.router_solicitations = 0 		
net.ipv6.ip6frag_high_thresh = 262144 		
net.ipv6.ip6frag_low_thresh = 196608 		
net.ipv6.route.flush = 1 		
net.unix.max_dgram_qlen = 50 		
vm.dirty_background_ratio = 5 		
vm.dirty_ratio = 30 		t
vm.min_free_kbytes = 65535 		
vm.mmap_min_addr = 4096 		
vm.overcommit_ratio = 50 		
vm.swappiness = 10 		
```
```

# Configure TimeZone
config_timezone(){
   clear
   f_banner
   echo -e "\e[34m---------------------------------------------------------------------------------------------------------\e[00m"
   echo -e "\e[93m[+]\e[00m We will now Configure the TimeZone"
   echo -e "\e[34m---------------------------------------------------------------------------------------------------------\e[00m"
   echo ""
   sleep 10
   dpkg-reconfigure tzdata
   say_done
}


#Ensure system accounts are non-login 

for user in `awk -F: '($3 < 1000) {print $1 }' /etc/passwd`; do
  if [ $user != "root" ]; then
    usermod -L $user
  if [ $user != "sync" ] && [ $user != "shutdown" ] && [ $user != "halt" ]; then
    usermod -s /usr/sbin/nologin $user
  fi
  fi
done

#Ensure default group for the root account is GID 0 

usermod -g 0 root

#Ensure default user umask is 027 or more restrictive 

sed -i s/umask\ 022/umask\ 027/g /etc/init.d/rc
`
# Create an Ed25519 key with ssh-keygen instead of using RSA :

```

```

 echo tty1 > /etc/securetty
    chmod 0600 /etc/securetty
    chmod 700 /root
    chmod 600 /boot/grub/grub.cfg
    #Remove AT and Restrict Cron
    apt purge at
    apt install -y libpam-cracklib
    echo ""
    echo " Securing Cron "
    spinner
    touch /etc/cron.allow
    chmod 600 /etc/cron.allow
    awk -F: '{print $1}' /etc/passwd | grep -v root > /etc/cron.deny
    echo ""
    echo -n " Do you want to Disable USB Support for this Server? (y/n): " ; read usb_answer
    if [ "$usb_answer" == "y" ]; then
       echo ""
       echo "Disabling USB Support"
       spinner
       echo "blacklist usb-storage" | sudo tee -a /etc/modprobe.d/blacklist.conf
       update-initramfs -u



Open `/etc/pam.d/system-auth` using any text editor and add the following line:

`/lib/security/$ISA/pam_cracklib.so retry=3 minlen=8 lcredit=-1 ucredit=-2 dcredit=-2 ocredit=-1`

* Linux will hash the password to avoid saving it in cleartext so, you need to make sure to define a secure password hashing algorithm SHA512.

* Another interesting functionality is to lock the account after five failed attempts. To make this happen, you need to open the file “/etc/pam.d/password-auth” and add the following lines:

```
auth required pam_env.so 
auth required pam_faillock.so preauth audit silent deny=5 unlock_time=604800 
auth [success=1 default=bad] pam_unix.so 
auth [default=die] pam_faillock.so authfail audit deny=5 unlock_time=604800 
auth sufficient pam_faillock.so authsucc audit deny=5 unlock_time=604800 
auth required pam_deny.so
```

* We’re not done yet; one additional step is needed. Open the file “/etc/pam.d/system-auth” and make sure you have the following lines added:

```
auth required pam_env.so 
auth required pam_faillock.so preauth audit silent deny=5 unlock_time=604800 
auth [success=1 default=bad] pam_unix.so 
auth [default=die] pam_faillock.so authfail audit deny=5 unlock_time=604800 
auth sufficient pam_faillock.so authsucc audit deny=5 unlock_time=604800 
auth required pam_deny.so
```

* After five failed attempts, only an administrator can unlock the account by using the following command:

# `/usr/sbin/faillock --user <userlocked*   --reset`



#!/bin/bash 
for user in `awk -F: '($3 < 500) {print $1 }' /etc/passwd`; do
if [ $user != "root" ] 
then 
/usr/sbin/usermod -L $user 
if [ $user != "sync" ] && [ $user != "shutdown" ] && [ $user != "halt" ] 
then /usr/sbin/usermod -s /sbin/nologin $user 
fi 
fi 
done



`nano /etc/modprobe.d/blacklist.conf`

* When the file opens, then add the following line at the end of the file (save and close):

`blacklist usb_storage`

* After this, open the rc.local file:

`nano /etc/rc.local`

* Finally, add the following two lines:

```
modprobe -r usb_storage
exit 0
```
```
```
#set some audit rules
find / -xdev \( -perm -4000 -o -perm -2000 \) -type f | awk '{print \
"-a always,exit -F path=" $1 " -F perm=x -F auid>=1000 -F auid!=4294967295 \
-k privileged" } ' >> /etc/audit/audit.rules

echo " " >> /etc/audit/audit.rules
echo "#End of Audit Rules" >> /etc/audit/audit.rules
echo "-e 2" >>/etc/audit/audit.rules

cp /etc/audit/audit.rules /etc/audit/rules.d/audit.rules
```
````

# Removing root user access, tty and login capabilities.

```
	sed -i "/SINGLE/s/sushell/sulogin/" /etc/sysconfig/init 
	sed -i "/SINGLE/s/sushell/sulogin/" /etc/sysconfig/init 
	
	#Default /etc/passwd for root

	root:x:0:0:root:/root:/bin/bash
```

* After disabling root login

`root:x:0:0:root:/root:/sbin/nologin`

* Can be a solutions combined with locking + chattr

```
passwd -l  root
sed -i -e 's/^root::/root:!:/' /etc/shadow
`
useradd -D -f 30

#Ensure system accounts are non-login (Scored)

for user in `awk -F: '($3 < 1000) {print $1 }' /etc/passwd`; do
  if [ $user != "root" ]; then
    usermod -L $user
  if [ $user != "sync" ] && [ $user != "shutdown" ] && [ $user != "halt" ]; then
    usermod -s /usr/sbin/nologin $user
  fi
  fi
done

#Ensure default group for the root account is GID 0 

usermod -g 0 root

#5.4.4 Ensure default user umask is 027 or more restrictive

sed -i s/umask\ 022/umask\ 027/g /etc/init.d/rc
```
##############################################################################################################``

```
echo *  /etc/secu
	[atmos@privy ~]$ sudo reboot
	[sudo] password for atmos:
	atmosis not in the sudoers file.  This incident will be reported.
```

* However after running ‘visudo’ and editing the sudoers file as below, this becomes possible.

```
atmos     ALL=/usr/sbin/reboot
retty

usermod -aG wheel $USER
sed -i '/%wheel/s/^# //' /etc/sudoers
or

mkdir -p ~/.ssh && sudo chmod -R 700 ~/.ssh/

From your local computer:

scp ~/.ssh/id_rsa.pub example_user@203.0.113.10:~/.ssh/authorized_keys

mkdir ~/.ssh; nano ~/.ssh/authorized_keys
mkdir -p ~/.ssh && sudo chmod -R 700 ~/.ssh/
```

$ sudo iptables --flush  # start again
$  do iptables --new-chain RATE-LIMIT
$ sudo iptables --append RATE-LIMIT \
    --match hashlimit \
    --hashlimit-upto 50/sec \
    --hashlimit-burst 20 \
    --hashlimit-name conn_rate_limit \
    --jump ACCEPT
$ sudo iptables --append RATE-LIMIT --jump DROP



# visudo can be configured to use an editor other than vi if desired. Edit /etc/sudoers using visudo:

```
  visudo /etc/sudoers
  Uncomment to allow members of group wheel to execute any command
  %wheel ALL=(ALL) ALL
```

* Lock the root account

The output of passwd -S root reveals how P is changed to L:
```
  $ sudo passwd -S root
  root P 03/27/2016 0 99999 7 -1
```

```  
  $ sudo passwd -dl root
  passwd: password expiry information changed.
```

```
  $ sudo passwd -S root
  root L 03/27/2016 0 99999 7 -1
```

* Set the root account shell to bash, the status can be viewed in `/etc/passwd`:

  `$ sudo usermod --shell /bin/bash root`

Set up sulogin for Grub rescue mode to allow operation without a root account password

Configure runit to use the sulogin -e option. Create the file /etc/sv/sulogin/conf with this content:

  OPTS="-e"

The conf file will be read by /etc/sv/sulogin/run.

Or if OPTS is not supported in the run file, edit the last line of /etc/sv/sulogin/run to this (although it will be overwritten on subsequent updates of runit and will need to be edited again):

  exec setsid sulogin -e < $tty * $tty 2* &1

This means if there is no root password, rescue mode boots to a root terminal which doesn't require a password. This is potentially insecure if the terminal can be physically accessed by others, although there are numerous other security issues in that situation. If a root password is set, then it will still be requested.

The root default environment in rescue mode could be lacking some elements for normal operation as displayed by the env command:

  $ env
  SHELL=/bin/bash
  USER=root
  PATH=/usr/bin:/usr/sbin
  PWD=/root
  SHLVL=1
  HOME=/root
  LOGNAME=root
  _=/usr/bin/env

This can be amended as desired by creating or editing /root/.bashrc, e.g.:

  # .bashrc

  # If not running interactively, don't do anything
  [[ $- != *i* ]] && return

  alias ls='ls --color=auto'
  PS1='[\u@\h \W]\$ '
  export PAGER=less
  export EDITOR=nano
  export TERM=xterm
  export PATH=/usr/local/sbin:/usr/local/bin:/usr/bin:/usr/sbin:/sbin:/bin

 find world writables files on the server

find /dir -xdev -type d \( -perm -0002 -a ! -perm -1000 \) -print
files without owner
find /dir -xdev \( -nouser -o -nogroup \) -print

lockdown cronjob 

echo ALL * * /etc/cron.deny

#Make a backup of OpenSSH server's configuration file /etc/ssh/sshd_config and remove comments to make it easier to read:

    sudo cp --preserve /etc/ssh/sshd_config /etc/ssh/sshd_config.$(date +"%Y%m%d%H%M%S")
    sudo sed -i -r -e '/^#|^$/ d' /etc/ssh/sshd_config

Create a group:

sudo groupadd sshusers

Add account(s) to the group:

sudo usermod -a -G sshusers $USER

port 22
addressfamily any
listenaddress [::]:22
listenaddress 0.0.0.0:22
usepam yes
logingracetime 30
x11displayoffset 10
maxauthtries 2
maxsessions 2
clientaliveinterval 300
clientalivecountmax 0
streamlocalbindmask 0177
permitrootlogin no
ignorerhosts yes
ignoreuserknownhosts no
hostbasedauthentication no
HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key

KexAlgorithms curve25519-sha256@libssh.org,ecdh-sha2-nistp521,ecdh-sha2-nistp384,ecdh-sha2-nistp256,diffie-hellman-group-exchange-sha256
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
```
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,umac-128@openssh.com

# LogLevel VERBOSE logs user's key fingerprint on login. Needed to have a clear audit track of which key was using to log in.
LogLevel VERBOSE

# Use kernel sandbox mechanisms where possible in unprivileged processes
# Systrace on OpenBSD, Seccomp on Linux, seatbelt on MacOSX/Darwin, rlimit elsewhere.
# Note: This setting is deprecated in OpenSSH 7.5 (https://www.openssh.com/txt/release-7.5)
UsePrivilegeSeparation sandbox

########################################################################################################
# end settings from https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67 as of 2019-01-01
########################################################################################################

# don't let users set environment variables
PermitUserEnvironment no

# Log sftp level file access (read/write/etc.) that would not be easily logged otherwise.
Subsystem sftp  internal-sftp -f AUTHPRIV -l INFO

# only use the newer, more secure protocol
Protocol 2

# disable X11 forwarding as X11 is very insecure
# you really shouldn't be running X on a server anyway
X11Forwarding no

# disable port forwarding
AllowTcpForwarding no
AllowStreamLocalForwarding no
GatewayPorts no
PermitTunnel no

# don't allow login if the account has an empty password
PermitEmptyPasswords no

# ignore .rhosts and .shosts
IgnoreRhosts yes

# verify hostname matches IP
UseDNS no

Compression no
TCPKeepAlive no
AllowAgentForwarding no
PermitRootLogin no

# don't allow .rhosts or /etc/hosts.equiv
HostbasedAuthentication no
subsystem sftp internal-sftp -f AUTHPRIV -l INFO
maxstartups 2:30:2
permittunnel no
ipqos lowdelay throughput
rekeylimit 0 0
permitopen any
```
```
sudo cp --preserve /etc/ssh/moduli /etc/ssh/moduli.$(date +"%Y%m%d%H%M%S")

Remove short moduli:

sudo awk '$5 * = 3071' /etc/ssh/moduli | sudo tee /etc/ssh/moduli.tmp
sudo mv /etc/ssh/moduli.tmp /etc/ssh/moduli

sudo sed -i -r -e "s/^(password\s+requisite\s+pam_pwquality.so)(.*)$/# \1\2         # commented by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")\n\1 retry=3 minlen=10 difok=3 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1 maxrepeat=3 gecoschec         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")/" /etc/pam.d/common-password

```
sudo psad -R
sudo psad --sig-update
sudo psad -H
sudo cp --preserve /etc/psad/psad.conf /etc/psad/psad.conf.$(date +"%Y%m%d%H%M%S")
sudo apt install aide
sudo cp -p /etc/default/aide /etc/default/aide.$(date +"%Y%m%d%H%M%S")
sudo cp -pr /etc/aide /etc/aide.$(date +"%Y%m%d%H%M%S")
sudo aideinit
sudo aide.wrapper --check
````

# if you are not sure the change worked , verify the configuration with this
```
sudo touch /etc/test.sh
sudo touch /root/test.sh
sudo aide.wrapper --check
sudo rm /etc/test.sh
sudo rm /root/test.sh
sudo aideinit -y -f

```
Ensure the following are set in /etc/pam.d/other:

    auth  required pam_deny.so
    auth   required pam_warn.so
    account  required pam_deny.so
    account  required pam_warn.so
    password  required pam_deny.so
    password  required pam_warn.so
    session  required pam_deny.so
    session  required pam_warn.so
    session  required pam_deny.so

Warn will report alerts to syslog.

To require strong passwords, in compliance with section 5.18 of the Information Resources Use and Security Policy:

For RHEL 6:

In /etc/pam.d/system-auth, add or change the file as required to read:
password   required     pam_cracklib.so retry=3 difok=5 minlen=8 lcredit=-1 dcredit=-1 ocredit=-1
password   sufficient   pam_unix.so sha512 shadow nullok try_first_pass use_authtok remember=10
password   required     pam_deny.so
password   required     pam_warn.so

 

For RHEL 7:

In /etc/security/pwquality.conf, add:
difok = 5
minlen = 8
minclass = 1
maxrepeat = 0
maxclassrepeat = 0
lcredit = -1
ucredit = 0
dcredit = -1
ocredit = -1
gecoscheck = 1

 

In /etc/pam.d/system-auth, add or change the file as required to read:
password    required    pam_pwquality.so try_first_pass local_users_only retry=3 authtok_type=
password    sufficient  pam_unix.so sha512 shadow try_first_pass use_authtok remember=10
password    required    pam_deny.so


#kernel hardening 

#disabling compiller
chmod 000 /usr/bin/as >/dev/null 2>&1
    chmod 000 /usr/bin/byacc >/dev/null 2>&1
    chmod 000 /usr/bin/yacc >/dev/null 2>&1
    chmod 000 /usr/bin/bcc >/dev/null 2>&1
    chmod 000 /usr/bin/kgcc >/dev/null 2>&1
    chmod 000 /usr/bin/cc >/dev/null 2>&1
    chmod 000 /usr/bin/gcc >/dev/null 2>&1
    chmod 000 /usr/bin/*c++ >/dev/null 2>&1
    chmod 000 /usr/bin/*g++ >/dev/null 2>&1



  chmod 750 /etc/apache2/conf* >/dev/null 2>&1
     chmod 511 /usr/sbin/apache2 >/dev/null 2>&1
     chmod 750 /var/log/apache2/ >/dev/null 2>&1
     chmod 640 /etc/apache2/conf-available/* >/dev/null 2>&1
     chmod 640 /etc/apache2/conf-enabled/* >/dev/null 2>&1
     chmod 640 /etc/apache2/apache2.conf >/dev/null 2>&1



Make sure no files have no owner specified

    find /dir -xdev \( -nouser -o -nogroup \) -print

Verify no files are world-writeable

    find /dir -xdev -type d \( -perm -0002 -a ! -perm -1000 \) -print

/etc/pam.d/system-login

auth optional pam_faildelay.so delay=4000000


/etc/pam.d/system-login

auth required pam_tally2.so deny=3 unlock_time=600 onerr=succeed file=/var/log/tallylog

````
#Artillery setup
````
 git clone https://github.com/BinaryDefense/artillery
    cd artillery/
    python setup.py
    cd ..
    echo ""
    echo "Setting Iptable rules for artillery"
    spinner
    for port in 22 1433 8080 21 5900 53 110 1723 1337 10000 5800 44443 16993; do
      echo "iptables -A INPUT -p tcp -m tcp --dport $port -j ACCEPT" >> /etc/init.d/iptables.sh
    done
    echo ""
    echo "Artillery configuration file is /var/artillery/config"
```

/etc/security/limits.conf
* soft nproc 100
* hard nproc 200
* soft nofile 100000
* hard nofile 100000


if you added hardening to /proc in /etc/fstab For user sessions to work correctly, an exception needs to be added for systemd-logind:

/etc/systemd/system/systemd-logind.service.d/hidepid.conf

[Service]
SupplementaryGroups=proc


# vim: filetype=conf:




If you are using Bash or Zsh, you can set TMOUT for an automatic logout from shells after a timeout.

For example, the following will automatically log out from virtual consoles (but not terminal emulators in X11):

/etc/profile.d/shell-timeout.sh

TMOUT="$(( 60*10 ))";
[ -z "$DISPLAY" ] && export TMOUT;
case $( /usr/bin/tty ) in
	/dev/tty[0-9]*) export TMOUT;;
esac

If you really want EVERY Bash/Zsh prompt (even within X) to timeout, use:

$ export TMOUT="$(( 60*10 ))";


sudo cp --preserve /etc/exim4/exim4.conf.template /etc/exim4/exim4.conf.template.$(date +"%Y%m%d%H%M%S")
cat << EOF | sudo tee /etc/exim4/exim4.conf.localmacros
MAIN_TLS_ENABLE = 1
REMOTE_SMTP_SMARTHOST_HOSTS_REQUIRE_TLS = *
TLS_ON_CONNECT_PORTS = 465
REQUIRE_PROTOCOL = smtps
IGNORE_SMTP_LINE_LENGTH_LIMIT = true
EOF


sudo sed -i -r -e "/\.ifdef MAIN_TLS_ENABLE/ a # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")\n.ifdef TLS_ON_CONNECT_PORTS\n    tls_on_connect_ports
 = TLS_ON_CONNECT_PORTS\n.endif\n# end add" /etc/exim4/exim4.conf.template


cat << EOF | sudo tee /etc/ufw/applications.d/smtptls
[SMTPTLS]
title=SMTP through TLS
description=This opens up the TLS port 465 for use with SMPT to send e-mails.
ports=465/tcp
EOF

sudo ufw allow out smtptls comment 'open TLS port 465 for use with SMPT to send e-mails'
cat << EOF | sudo tee /etc/rsyslog.d/10-iptables.conf
:msg, contains, "[IPTABLES] " /var/log/iptables.log
& stop
EOF

sudo sed -i -r -e "s/^(IPT_SYSLOG_FILE\s+)([^;]+)(;)$/# \1\2\3       # commented by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")\n\1\/var\/log\/iptables.log\3       # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")/" /etc/psad/psad.conf
sudo psad -R
sudo psad --sig-update
sudo psad -H
sudo service rsyslog restart

echo -e ""
echo -e "Setting Sticky bit on all world-writable directories"




 change /etc/rc.securelevel so that the securelevel is 2. Then go through your files and chflags -R schg them. I would do this for most of /etc, all of /bin,/sbin,/usr,/bsd,/boot and sappend on some other files/directories like /root and /altroot and on key logs in /var/log. You may need to hand-tune log rotation. 
d.) use TCP Wrappers (/etc/hosts.allow,/etc/hosts.deny). /etc/hosts.deny should read: ALL: ALL. Then figure out what you will allow. Also consider turning off inetd entirely by putting inetd_flags=NO in /etc/rc.conf.local There's a way to boobytrap TCP Wrappers that's explained in the man page, but I haven't done it yet.

e.) use mtree -cK sha1digest *  snapshot_of_filesystem__on_date once you have everything set up. Then cksum -a sha1 that file. ...as explained in the mtree man page. Make it a cron job, and write a script to diff your snapshots. Also keep the main snapshot offline. This can alert you if key files have been tampered with or accessed by someone other than you or your machine. So it's sort of like a host-based IDS.

f.) deny root login and port forwarding/X11 forwarding in /etc/ssh/sshd_config, especially if you are running sshd!

g.) in /etc/fstab mount /usr ro, and /tmp,/var,/home with noexec Consider whether your user can log into an rksh shell.

cat /dev/srandom | tr -dc [:print:] | fold -w PWD_LENGTH | head -n NUM_OF_PWDS
