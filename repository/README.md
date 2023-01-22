<h1> Create Repository </h1>
<b> Ansible repo </b>
<pre>
 hammer repository create --organization-id 3 \
    --product "CentOS 7 Linux x86_64" \
    --name "Ansible x86_64" \
    --label "Ansible_x86_64" \
    --content-type "yum" \
    --download-policy "on_demand" \
    --gpg-key "RPM-GPG-KEY-CentOS-7" \
    --url "http://mirror.centos.org/centos-7/7/configmanagement/x86_64/ansible29/" \
    --mirror-on-sync "no"
<b> Centos 7 repo </b>
hammer repository create --organization-id 3 \
   --product "CentOS 7 Linux x86_64" \
   --name "CentOS 7 OS x86_64" \
   --label "CentOS_7_OS_x86_64" \
   --content-type "yum" \
   --download-policy "on_demand" \
   --gpg-key "RPM-GPG-KEY-CentOS-7" \
   --url "http://mirror.centos.org/centos-7/7/os/x86_64/" \
   --mirror-on-sync "no"
<b> Centos 7 Extra repo </b>
hammer repository create --organization-id 3 \
   --product "CentOS 7 Linux x86_64" \
   --name "CentOS 7 Extra x86_64" \
   --label "CentOS_7_Extra_x86_64" \
   --content-type "yum" \
   --download-policy "on_demand" \
   --gpg-key "RPM-GPG-KEY-CentOS-7" \
   --url "http://mirror.centos.org/centos-7/7/extras/x86_64/" \
   --mirror-on-sync "no"
<b> Centos 7 Updates </b>
hammer repository create --organization-id 3 \
   --product "CentOS 7 Linux x86_64" \
   --name "CentOS 7 Updates x86_64" \
   --label "CentOS_7_Updates_x86_64" \
   --content-type "yum" \
   --download-policy "on_demand" \
   --gpg-key "RPM-GPG-KEY-CentOS-7" \
   --url "http://mirror.centos.org/centos-7/7/updates/x86_64/" \
   --mirror-on-sync "no"

<h1> Create repositories </h1>
<b>root@foreman Katello-hammer]# hammer organization list</b>
---|----------------------|----------------------|-------------|---------------------
ID | TITLE                | NAME                 | DESCRIPTION | LABEL
---|----------------------|----------------------|-------------|---------------------
1  | Default Organization | Default Organization |             | Default_Organization
---|----------------------|----------------------|-------------|---------------------
[root@foreman Katello-hammer]# hammer organization list
---|----------------------|----------------------|-------------|---------------------
ID | TITLE                | NAME                 | DESCRIPTION | LABEL
---|----------------------|----------------------|-------------|---------------------
1  | Default Organization | Default Organization |             | Default_Organization
3  | operations           | operations           | operations  | operations
---|----------------------|----------------------|-------------|---------------------
[root@foreman Katello-hammer]#
<b>[root@foreman Katello-hammer]# hammer product create --organization-id 3 --name "CentOS 7 Linux x86_64" --description "Repository for CentOS 7 Linux"</b>
Product created.
[root@foreman Katello-hammer]#
[root@foreman Katello-hammer]# mkdir -p /etc/pki/rpm-gpg/import
[root@foreman Katello-hammer]#  wget -P /etc/pki/rpm-gpg/import/ http://mirror.centos.org/centos-7/7/os/x86_64/RPM-GPG-KEY-CentOS-7
--2022-02-06 11:43:51--  http://mirror.centos.org/centos-7/7/os/x86_64/RPM-GPG-KEY-CentOS-7
Resolving mirror.centos.org (mirror.centos.org)... 72.29.84.207, 2605:9000:401:103::2
Connecting to mirror.centos.org (mirror.centos.org)|72.29.84.207|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1690 (1.7K)
Saving to: ‘/etc/pki/rpm-gpg/import/RPM-GPG-KEY-CentOS-7’
100%[==================================================================================================>] 1,690       --.-K/s   in 0.001s
2022-02-06 11:43:52 (2.74 MB/s) - ‘/etc/pki/rpm-gpg/import/RPM-GPG-KEY-CentOS-7’ saved [1690/1690]
[root@foreman Katello-hammer]#
[root@foreman Katello-hammer]# ll
total 36
-rw-r--r-- 1 root root  1108 Feb  6 11:02 commands1.md
-rw-r--r-- 1 root root  2967 Feb  6 10:57 katallo-nates.2018-11-05
-rw-r--r-- 1 root root  5380 Feb  6 10:57 katello-notes34.txt
-rw-r--r-- 1 root root 16377 Feb  6 10:57 katello-notes.txt
-rw-r--r-- 1 root root    21 Feb  6 10:57 README.md
[root@foreman Katello-hammer]# cd /etc/pki/rpm-gpg/
[root@foreman rpm-gpg]# ll
total 40
drwxr-xr-x 2 root root   34 Feb  6 11:43 import
-rw-r--r-- 1 root root 1710 Feb  4  2021 RPM-GPG-KEY-2025-04-06-puppet6-release
-rw-r--r-- 1 root root 1690 Nov 23  2020 RPM-GPG-KEY-CentOS-7
-rw-r--r-- 1 root root 1004 Nov 23  2020 RPM-GPG-KEY-CentOS-Debug-7
-rw-r--r-- 1 root root 1057 Oct 29  2018 RPM-GPG-KEY-CentOS-SIG-SCLo
-rw-r--r-- 1 root root 1690 Nov 23  2020 RPM-GPG-KEY-CentOS-Testing-7
-rw-r--r-- 1 root root 1662 Sep  4 10:37 RPM-GPG-KEY-EPEL-7
-rw-r--r-- 1 root root 1692 May 24  2020 RPM-GPG-KEY-foreman
-rw-r--r-- 1 root root 1692 May 24  2020 RPM-GPG-KEY-foreman-client
-rw-r--r-- 1 root root 1038 May 24  2020 RPM-GPG-KEY-foreman-rails
-rw-r--r-- 1 root root 1676 Feb  4  2021 RPM-GPG-KEY-puppet6-release
<b>[root@foreman rpm-gpg]# hammer gpg create --organization-id 3 --key "RPM-GPG-KEY-CentOS-7" --name "RPM-GPG-KEY-CentOS-7" </b>
GPG Key created.
[root@foreman rpm-gpg]#
[root@foreman rpm-gpg]# cat repo.sh
hammer repository create --organization-id 3 \
   --product "CentOS 7 Linux x86_64" \
   --name "CentOS 7 OS x86_64" \
   --label "CentOS_7_OS_x86_64" \
   --content-type "yum" \
   --download-policy "on_demand" \
   --gpg-key "RPM-GPG-KEY-CentOS-7" \
   --url "http://mirror.centos.org/centos-7/7/os/x86_64/" \
   --mirror-on-sync "no"
[root@foreman rpm-gpg]# bash repo.sh
Repository created.
[root@foreman rpm-gpg]#
[root@foreman rpm-gpg]# bash extra-repo.sh
Repository created.
[root@foreman rpm-gpg]# cat extra-repo.sh
 hammer repository create --organization-id 3 \
   --product "CentOS 7 Linux x86_64" \
   --name "CentOS 7 Extra x86_64" \
   --label "CentOS_7_Extra_x86_64" \
   --content-type "yum" \
   --download-policy "on_demand" \
   --gpg-key "RPM-GPG-KEY-CentOS-7" \
   --url "http://mirror.centos.org/centos-7/7/extras/x86_64/" \
   --mirror-on-sync "no"
[root@foreman rpm-gpg]#
[root@foreman rpm-gpg]# cat updates-repo.sh
 hammer repository create --organization-id 3 \
   --product "CentOS 7 Linux x86_64" \
   --name "CentOS 7 Updates x86_64" \
   --label "CentOS_7_Updates_x86_64" \
   --content-type "yum" \
   --download-policy "on_demand" \
   --gpg-key "RPM-GPG-KEY-CentOS-7" \
   --url "http://mirror.centos.org/centos-7/7/updates/x86_64/" \
   --mirror-on-sync "no"
[root@foreman rpm-gpg]# bash updates-repo.sh
Repository created.
[root@foreman rpm-gpg]#
[root@foreman Katello-hammer]# cat Ansibe.sh
 hammer repository create --organization-id 3 \
    --product "CentOS 7 Linux x86_64" \
    --name "Ansible x86_64" \
    --label "Ansible_x86_64" \
    --content-type "yum" \
    --download-policy "on_demand" \
    --gpg-key "RPM-GPG-KEY-CentOS-7" \
    --url "http://mirror.centos.org/centos-7/7/configmanagement/x86_64/ansible29/" \
    --mirror-on-sync "no"
[root@foreman Katello-hammer]# bash Ansibe.sh
Repository created.
[root@foreman Katello-hammer]#
<b>[root@foreman Katello-hammer]#  hammer repository list --organization-id 3 --product "CentOS 7 Linux x86_64" </b>
---|-------------------------|-----------------------|--------------|-----------------------------------------------------------------------
ID | NAME                    | PRODUCT               | CONTENT TYPE | URL
---|-------------------------|-----------------------|--------------|-----------------------------------------------------------------------
4  | Ansible x86_64          | CentOS 7 Linux x86_64 | yum          | http://mirror.centos.org/centos-7/7/configmanagement/x86_64/ansible29/
2  | CentOS 7 Extra x86_64   | CentOS 7 Linux x86_64 | yum          | http://mirror.centos.org/centos-7/7/extras/x86_64/
1  | CentOS 7 OS x86_64      | CentOS 7 Linux x86_64 | yum          | http://mirror.centos.org/centos-7/7/os/x86_64/
3  | CentOS 7 Updates x86_64 | CentOS 7 Linux x86_64 | yum          | http://mirror.centos.org/centos-7/7/updates/x86_64/
---|-------------------------|-----------------------|--------------|-----------------------------------------------------------------------
[root@foreman Katello-hammer]#
</pre>
