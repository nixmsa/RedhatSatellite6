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



</pre>
