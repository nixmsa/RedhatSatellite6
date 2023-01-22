<pre>
<h1> Hammer Commands </h1>
<b> User Information </b>
[root@foreman Katello-hammer]# hammer user list ---|--------|----------------|------------------|-------|---------------------|--------------
ID 	LOGIN 	NAME 	EMAIL 	ADMIN 	LAST LOGIN 	AUTHORIZED BY
4 	admin 	Admin User 	root@example.com 	yes 	2022/02/06 18:49:24 	Internal
5 	mahsan 	Muhammad Ahsan 		no 		Internal
--- 	-------- 	---------------- 	------------------ 	------- 	--------------------- 	--------------
[root@foreman Katello-hammer]# 						
..
[root@foreman Katello-hammer]# hammer organization list ---|----------------------|----------------------|-------------|---------------------
ID 	TITLE 	NAME 	DESCRIPTION 	LABEL
1 	Default Organization 	Default Organization 		Default_Organization
--- 	---------------------- 	---------------------- 	------------- 	---------------------
root@katello# hammer auth status
You are logged in as 'admin'
..
root@katello# hammer organization list
---|----------------------|-------------|----------------------|------------
ID | NAME                 | DESCRIPTION | LABEL                | DESCRIPTION
---|----------------------|-------------|----------------------|------------
1  | Default Organization |             | Default_Organization |
3  | MSA                  | MSA ORG     | MSA                  | MSA ORG
---|----------------------|-------------|----------------------|------------
root@katello# hammer --csv --csv-separator ";" organization list
Id;Name;Description;Label;Description
1;Default Organization;;Default_Organization;
3;MSA;MSA ORG;MSA;MSA ORG
root@katello#
..
t@katello# hammer ping
candlepin:
    Status:          ok
    Server Response: Duration: 25ms
candlepin_auth:
    Status:          ok
    Server Response: Duration: 29ms
pulp:
    Status:          FAIL
    Server Response:
pulp_auth:
    Status: FAIL
foreman_tasks:
    Status:          ok
    Server Response: Duration: 23ms
root@katello#
..
<b>@katello# hammer organization create --name "SAM" --description "SAM ORG Surrey"</b>
Organization created
root@katello#
..
root@katello#  hammer organization list
---|----------------------|----------------|----------------------|---------------
ID | NAME                 | DESCRIPTION    | LABEL                | DESCRIPTION
---|----------------------|----------------|----------------------|---------------
1  | Default Organization |                | Default_Organization |
3  | MSA                  | MSA ORG        | MSA                  | MSA ORG
4  | SAM                  | SAM ORG Surrey | SAM                  | SAM ORG Surrey
---|----------------------|----------------|----------------------|---------------
root@katello#
..
<b>@katello# hammer location create --name surrey</b>
Location created
root@katello#  hammer location  list
---|------------------|------------
ID | NAME             | DESCRIPTION
---|------------------|------------
2  | Default Location |
5  | surrey           |
---|------------------|------------
root@katello#
..
r: Could not find organization, please set one of options --organization, --organization-label, --organization-id.
root@katello# hammer product list  --organization "SAM"
---|------|-------------|--------------|--------------|-----------
ID | NAME | DESCRIPTION | ORGANIZATION | REPOSITORIES | SYNC STATE
---|------|-------------|--------------|--------------|-----------
root@katello# hammer product list  --organization "MSA"
---|----------|----------------|--------------|--------------|------------------
ID | NAME     | DESCRIPTION    | ORGANIZATION | REPOSITORIES | SYNC STATE
---|----------|----------------|--------------|--------------|------------------
1  | CentOS 7 | Sync Base Repo | MSA          | 3            | Syncing Complete.
---|----------|----------------|--------------|--------------|------------------
root@katello#
..
<b>[root@sat6 katello38]# cat /root/.hammer/cli.modules.d/foreman.yml</b>
:foreman:
  # Credentials. You'll be asked for the interactively if you leave them blank here
#  :username: 'admin'
#  :password: '3osXJR4VUvAycido'
  :host: 'https://sat6.bc.ca'
  :username: 'admin'
  :password: 'tripod001'
  # Check API documentation cache status on each request
  :refresh_cache: false
  # API request timeout in seconds, set -1 for infinity
  :request_timeout: 120
[root@sat6 katello38]#
..
[root@sat6 katello38]# hammer organization list
---|----------------------|----------------------|------------------------------|----------------------|-----------------------------
ID | TITLE                | NAME                 | DESCRIPTION                  | LABEL                | DESCRIPTION
---|----------------------|----------------------|------------------------------|----------------------|-----------------------------
1  | Default Organization | Default Organization |                              | Default_Organization |
3  | MSA                  | MSA                  | MSA groups of company canada | MSA                  | MSA groups of company canada
---|----------------------|----------------------|------------------------------|----------------------|-----------------------------
[root@sat6 katello38]#
[root@sat6 katello38]# hammer organization list
---|----------------------|----------------------|------------------------------|----------------------|-----------------------------
ID | TITLE                | NAME                 | DESCRIPTION                  | LABEL                | DESCRIPTION
---|----------------------|----------------------|------------------------------|----------------------|-----------------------------
1  | Default Organization | Default Organization |                              | Default_Organization |
3  | MSA                  | MSA                  | MSA groups of company canada | MSA                  | MSA groups of company canada
4  | Surrey               | Surrey               | Surrey BC                    | Surrey               | Surrey BC
---|----------------------|----------------------|------------------------------|----------------------|-----------------------------
[root@sat6 katello38]#
[root@sat6 katello38]# hammer sync-plan create --name "Daily Sync Plan" --interval daily --enabled true --organization "Surrey" --sync-date "2018-10-13"
Sync plan created.
[root@sat6 katello38]#
[root@sat6 katello38]# hammer sync-plan list --organization "Surrey"
---|-----------------|---------------------|----------|--------
ID | NAME            | START DATE          | INTERVAL | ENABLED
---|-----------------|---------------------|----------|--------
1  | Daily Sync Plan | 2018/10/13 00:00:00 | daily    | yes
---|-----------------|---------------------|----------|--------
[root@sat6 katello38]#
..
<b> hammer product create   --name "el7_repos"   --description "Various repositories to use with CentOS 7"  --organization=Surrey</b>
Product created.
..
[root@sat6 katello38]# hammer product list --organization "Surrey"
---|-----------|-------------------------------------------|--------------|--------------|-----------
ID | NAME      | DESCRIPTION                               | ORGANIZATION | REPOSITORIES | SYNC STATE
---|-----------|-------------------------------------------|--------------|--------------|-----------
1  | el7_repos | Various repositories to use with CentOS 7 | Surrey       | 0            |
---|-----------|-------------------------------------------|--------------|--------------|-----------
[root@sat6 katello38]#
..
[root@sat6 katello38]# mkdir /etc/pki/rpm-gpg/import/
[root@sat6 katello38]# cd /etc/pki/rpm-gpg/import/
[root@sat6 import]# wget http://mirror.centos.org/centos/7/os/x86_64/RPM-GPG-KEY-CentOS-7
--2018-10-13 08:25:06--  http://mirror.centos.org/centos/7/os/x86_64/RPM-GPG-KEY-CentOS-7
Resolving mirror.centos.org (mirror.centos.org)... 64.251.25.238, 2607:f7c0:1:ea30:4527:5180:0:19
Connecting to mirror.centos.org (mirror.centos.org)|64.251.25.238|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1690 (1.7K)
Saving to: ‘RPM-GPG-KEY-CentOS-7’
100%[========================================================================================================================>] 1,690       --.-K/s   in 0s
2018-10-13 08:25:07 (107 MB/s) - ‘RPM-GPG-KEY-CentOS-7’ saved [1690/1690]
[root@sat6 import]# ls
RPM-GPG-KEY-CentOS-7
[root@sat6 import]#
[root@sat6 import]# cat RPM-GPG-KEY-CentOS-7
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1.4.5 (GNU/Linux)
mQINBFOn/0sBEADLDyZ+DQHkcTHDQSE0a0B2iYAEXwpPvs67cJ4tmhe/iMOyVMh9
Yw/vBIF8scm6T/vPN5fopsKiW9UsAhGKg0epC6y5ed+NAUHTEa6pSOdo7CyFDwtn
4HF61Esyb4gzPT6QiSr0zvdTtgYBRZjAEPFVu3Dio0oZ5UQZ7fzdZfeixMQ8VMTQ
4y4x5vik9B+cqmGiq9AW71ixlDYVWasgR093fXiD9NLT4DTtK+KLGYNjJ8eMRqfZ
Ws7g7C+9aEGHfsGZ/SxLOumx/GfiTloal0dnq8TC7XQ/JuNdB9qjoXzRF+faDUsj
WuvNSQEqUXW1dzJjBvroEvgTdfCJfRpIgOrc256qvDMp1SxchMFltPlo5mbSMKu1
x1p4UkAzx543meMlRXOgx2/hnBm6H6L0FsSyDS6P224yF+30eeODD4Ju4BCyQ0jO
IpUxmUnApo/m0eRelI6TRl7jK6aGqSYUNhFBuFxSPKgKYBpFhVzRM63Jsvib82rY
438q3sIOUdxZY6pvMOWRkdUVoz7WBExTdx5NtGX4kdW5QtcQHM+2kht6sBnJsvcB
JYcYIwAUeA5vdRfwLKuZn6SgAUKdgeOtuf+cPR3/E68LZr784SlokiHLtQkfk98j
NXm6fJjXwJvwiM2IiFyg8aUwEEDX5U+QOCA0wYrgUQ/h8iathvBJKSc9jQARAQAB
tEJDZW50T1MtNyBLZXkgKENlbnRPUyA3IE9mZmljaWFsIFNpZ25pbmcgS2V5KSA8
c2VjdXJpdHlAY2VudG9zLm9yZz6JAjUEEwECAB8FAlOn/0sCGwMGCwkIBwMCBBUC
CAMDFgIBAh4BAheAAAoJECTGqKf0qA61TN0P/2730Th8cM+d1pEON7n0F1YiyxqG
QzwpC2Fhr2UIsXpi/lWTXIG6AlRvrajjFhw9HktYjlF4oMG032SnI0XPdmrN29lL
F+ee1ANdyvtkw4mMu2yQweVxU7Ku4oATPBvWRv+6pCQPTOMe5xPG0ZPjPGNiJ0xw
4Ns+f5Q6Gqm927oHXpylUQEmuHKsCp3dK/kZaxJOXsmq6syY1gbrLj2Anq0iWWP4
Tq8WMktUrTcc+zQ2pFR7ovEihK0Rvhmk6/N4+4JwAGijfhejxwNX8T6PCuYs5Jiv
hQvsI9FdIIlTP4XhFZ4N9ndnEwA4AH7tNBsmB3HEbLqUSmu2Rr8hGiT2Plc4Y9AO
aliW1kOMsZFYrX39krfRk2n2NXvieQJ/lw318gSGR67uckkz2ZekbCEpj/0mnHWD
3R6V7m95R6UYqjcw++Q5CtZ2tzmxomZTf42IGIKBbSVmIS75WY+cBULUx3PcZYHD
ZqAbB0Dl4MbdEH61kOI8EbN/TLl1i077r+9LXR1mOnlC3GLD03+XfY8eEBQf7137
YSMiW5r/5xwQk7xEcKlbZdmUJp3ZDTQBXT06vavvp3jlkqqH9QOE8ViZZ6aKQLqv
pL+4bs52jzuGwTMT7gOR5MzD+vT0fVS7Xm8MjOxvZgbHsAgzyFGlI1ggUQmU7lu3
uPNL0eRx4S1G4Jn5
=OGYX
-----END PGP PUBLIC KEY BLOCK-----
[root@sat6 import]#
[root@sat6 import]# hammer gpg create   --key "RPM-GPG-KEY-CentOS-7"   --name "RPM-GPG-KEY-CentOS-7"   --organization=Surrey
GPG Key created.
[root@sat6 import]#
[root@sat6 import]#  wget https://archive.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7Server
--2018-10-13 08:32:06--  https://archive.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7Server
Resolving archive.fedoraproject.org (archive.fedoraproject.org)... 209.132.181.24, 209.132.181.25, 209.132.181.23
Connecting to archive.fedoraproject.org (archive.fedoraproject.org)|209.132.181.24|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1662 (1.6K)
Saving to: ‘RPM-GPG-KEY-EPEL-7Server’
100%[========================================================================================================================>] 1,662       --.-K/s   in 0s
2018-10-13 08:32:06 (65.9 MB/s) - ‘RPM-GPG-KEY-EPEL-7Server’ saved [1662/1662]
[root@sat6 import]# ll
total 8
-rw-r--r-- 1 root root 1690 Dec  9  2015 RPM-GPG-KEY-CentOS-7
-rw-r--r-- 1 root root 1662 Jun 20  2014 RPM-GPG-KEY-EPEL-7Server
[root@sat6 import]#
<b># hammer gpg create \
>   --key "RPM-GPG-KEY-EPEL-7Server" \
>   --name "RPM-GPG-KEY-EPEL-7Server"  --organization=Surrey</b>
GPG Key created.
[root@sat6 import]#
ot@sat6 import]#  hammer gpg list --order ID --organization=Surrey
---|-------------------------
ID | NAME
---|-------------------------
1  | RPM-GPG-KEY-CentOS-7
2  | RPM-GPG-KEY-EPEL-7Server
---|-------------------------
[root@sat6 import]#
[root@sat6 import]# wget https://repo.mysql.com/RPM-GPG-KEY-mysql
--2018-10-13 08:51:47--  https://repo.mysql.com/RPM-GPG-KEY-mysql
Resolving repo.mysql.com (repo.mysql.com)... 23.59.157.177
Connecting to repo.mysql.com (repo.mysql.com)|23.59.157.177|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 27840 (27K) [text/plain]
Saving to: ‘RPM-GPG-KEY-mysql’
100%[========================================================================================================================>] 27,840      --.-K/s   in 0.008s
2018-10-13 08:51:49 (3.15 MB/s) - ‘RPM-GPG-KEY-mysql’ saved [27840/27840]
[root@sat6 import]# ll
total 36
-rw-r--r-- 1 root root  1690 Dec  9  2015 RPM-GPG-KEY-CentOS-7
-rw-r--r-- 1 root root  1662 Jun 20  2014 RPM-GPG-KEY-EPEL-7Server
-rw-r--r-- 1 root root 27840 Feb 20  2017 RPM-GPG-KEY-mysql
<b>[root@sat6 import]# hammer gpg create \
>   --key "RPM-GPG-KEY-mysql" \
>   --name "RPM-GPG-KEY-mysql"  --organization=Surrey</b>
GPG Key created.
[root@sat6 import]#
<b>[root@sat6 import]#  hammer gpg list --order ID --organization=Surrey</b>
---|-------------------------
ID | NAME
---|-------------------------
1  | RPM-GPG-KEY-CentOS-7
2  | RPM-GPG-KEY-EPEL-7Server
3  | RPM-GPG-KEY-mysql
---|-------------------------
[root@sat6 import]#
[root@sat6 import]# hammer repository create \
>   --product "el7_repos" \
>   --name "base_x86_64" \
>   --label "base_x86_64" \
>   --content-type "yum" \
>   --download-policy "on_demand" \
>   --gpg-key "RPM-GPG-KEY-CentOS-7" \
>   --url "http://mirror.centos.org/centos/7/os/x86_64/" \
>   --mirror-on-sync "no" --organization=Surrey
Repository created.
[root@sat6 import]#
ot@sat6 import]# hammer repository create   --product "el7_repos"   --name "extras_x86_64"   --label "extras_x86_64"   --content-type "yum"   --download-policy "on_demand"   --gpg-key "RPM-GPG-KEY-CentOS-7"   --url "http://mirror.centos.org/centos/7/extras/x86_64/" --organization=Surrey
Repository created.
[root@sat6 import]# hammer repository create \
>   --product "el7_repos" \
>   --name "mysql_57_x86_64" \
>   --label "mysql_57_x86_64" \
>   --content-type "yum" \
>   --download-policy "on_demand" \
>   --gpg-key "RPM-GPG-KEY-mysql" \
>   --url "https://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/" --organization=Surrey
Repository created.
[root@sat6 import]# hammer repository list --order ID
---|-----------------|-----------|--------------|------------------------------------------------------------
ID | NAME            | PRODUCT   | CONTENT TYPE | URL
---|-----------------|-----------|--------------|------------------------------------------------------------
1  | base_x86_64     | el7_repos | yum          | http://mirror.centos.org/centos/7/os/x86_64/
2  | extras_x86_64   | el7_repos | yum          | http://mirror.centos.org/centos/7/extras/x86_64/
3  | mysql_57_x86_64 | el7_repos | yum          | https://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/
---|-----------------|-----------|--------------|------------------------------------------------------------
[root@sat6 import]#
<b>[root@sat6 import]# hammer content-view create \
>   --name "el7_content" \
>   --description "Content view for CentOS 7"  --organization=Surrey</b>
Content view created.
[root@sat6 import]#
[root@sat6 import]# hammer product list
Error: Could not find organization, please set one of options --organization, --organization-label, --organization-id.
[root@sat6 import]# hammer product list  --organization=Surrey
---|-----------|-------------------------------------------|--------------|--------------|------------------
ID | NAME      | DESCRIPTION                               | ORGANIZATION | REPOSITORIES | SYNC STATE
---|-----------|-------------------------------------------|--------------|--------------|------------------
1  | el7_repos | Various repositories to use with CentOS 7 | Surrey       | 3            | Syncing Complete.
---|-----------|-------------------------------------------|--------------|--------------|------------------
[root@sat6 import]#
<b>[root@sat6 import]# for i in $(seq 1 16); do   hammer content-view add-repository   --name "el7_content"   --product "el7_repos"  --organization=Surrey    --repository-id "$i";   done </b>
The repository has been associated.
The repository has been associated.
The repository has been associated.
<b>
ot@sat6 import]# hammer lifecycle-environment create \
>   --name "stable" \
>   --label "stable" \
>   --prior "Library" --organization=Surrey </b>
Environment created.
[root@sat6 import]#
ot@sat6 import]#  hammer lifecycle-environment list
---|---------|--------
ID | NAME    | PRIOR
---|---------|--------
3  | Library |
2  | Library |
1  | Library |
4  | stable  | Library
---|---------|--------
[root@sat6 import]#
..
<b>[root@sat6 import]# hammer content-view publish \
>   --name "el7_content" \
>   --description "Publishing repositories"  --organization=Surrey</b>
[.........................................................................................................................................................] [100%]
{"content_view_id"=>4,
 "content_view_version_id"=>4,
 "composite_version_auto_published"=>[],
 "composite_view_publish_failed"=>[],
 "composite_auto_publish_task_id"=>[]}
[root@sat6 import]#
<b>[root@sat6 import]# hammer content-view version list</b>
---|-------------------------------|---------|-----------------------
ID | NAME                          | VERSION | LIFECYCLE ENVIRONMENTS
---|-------------------------------|---------|-----------------------
4  | el7_content 1.0               | 1.0     | Library
3  | Default Organization View 1.0 | 1.0     | Library
2  | Default Organization View 1.0 | 1.0     | Library
1  | Default Organization View 1.0 | 1.0     | Library
---|-------------------------------|---------|-----------------------
[root@sat6 import]#
<b>t@sat6 import]# hammer content-view version promote \
>   --content-view "el7_content" \
>   --version "1.0" \
>   --to-lifecycle-environment "stable"  --organization=Surrey</b>
[.........................................................................................................................................................] [100%]
[root@sat6 import]# hammer content-view version list
---|-------------------------------|---------|-----------------------
ID | NAME                          | VERSION | LIFECYCLE ENVIRONMENTS
---|-------------------------------|---------|-----------------------
4  | el7_content 1.0               | 1.0     | Library, stable
3  | Default Organization View 1.0 | 1.0     | Library
2  | Default Organization View 1.0 | 1.0     | Library
1  | Default Organization View 1.0 | 1.0     | Library
---|-------------------------------|---------|-----------------------
[root@sat6 import]#
<b>@sat6 import]# hammer activation-key create \
>   --name "el7-key" \
>   --description "Key to use with CentOS7" \
>   --lifecycle-environment "stable" \
>   --content-view "el7_content" \
>   --unlimited-hosts  --organization=Surrey </b>
Activation key created.
[root@sat6 import]# hammer activation-key list
Error: Could not find organization, please set one of options --organization, --organization-label, --organization-id.
[root@sat6 import]# hammer activation-key list --organization=Surrey
---|---------|----------------|-----------------------|-------------
ID | NAME    | HOST LIMIT     | LIFECYCLE ENVIRONMENT | CONTENT VIEW
---|---------|----------------|-----------------------|-------------
1  | el7-key | 0 of Unlimited | stable                | el7_content
---|---------|----------------|-----------------------|-------------
[root@sat6 import]#
ot@sat6 import]# hammer subscription list --organization=Surrey
---|----------------------------------|-----------|----------|----------|---------|---------|---------------------|-----------|---------
ID | UUID                             | NAME      | TYPE     | CONTRACT | ACCOUNT | SUPPORT | END DATE            | QUANTITY  | CONSUMED
---|----------------------------------|-----------|----------|----------|---------|---------|---------------------|-----------|---------
1  | 40280887666ddee801666e0066860005 | el7_repos | Physical |          |         |         | 2048/10/05 15:16:51 | Unlimited | 0
---|----------------------------------|-----------|----------|----------|---------|---------|---------------------|-----------|---------
[root@sat6 import]#
<b>[root@sat6 import]# hammer activation-key add-subscription \
>   --name "el7-key" \
>   --quantity "1" \
>   --subscription-id "1"  --organization=Surrey</b>
Subscription added to activation key.
[root@sat6 import]#
..
root@katello# vim /root/.hammer/cli.modules.d/foreman.yml
root@katello# hammer organization list
---|----------------------|-------------|----------------------|------------
ID | NAME                 | DESCRIPTION | LABEL                | DESCRIPTION
---|----------------------|-------------|----------------------|------------
1  | Default Organization |             | Default_Organization |
3  | MSA                  | MSA ORG     | MSA                  | MSA ORG
---|----------------------|-------------|----------------------|------------
root@katello#
t@katello# hammer sync-plan list --organization "MSA"
---|--------------------|---------------------|----------|--------
ID | NAME               | START DATE          | INTERVAL | ENABLED
---|--------------------|---------------------|----------|--------
1  | Centos_7_Sync_Plan | 2018/10/01 05:30:00 | weekly   | yes
---|--------------------|---------------------|----------|--------
root@katello#
..
t@katello#  hammer product list --organization "MSA"
---|----------|----------------|--------------|--------------|------------------
ID | NAME     | DESCRIPTION    | ORGANIZATION | REPOSITORIES | SYNC STATE
---|----------|----------------|--------------|--------------|------------------
1  | CentOS 7 | Sync Base Repo | MSA          | 3            | Syncing Complete.
---|----------|----------------|--------------|--------------|------------------
root@katello#
root@katello#  hammer gpg list --order ID --organization=MSA
---|-----------------
ID | NAME
---|-----------------
1  | Centos_7_GPG_Key
---|-----------------
root@katello#
root@katello# hammer repository list --order ID
---|----------------|----------|--------------|----------------------------------------------------
ID | NAME           | PRODUCT  | CONTENT TYPE | URL
---|----------------|----------|--------------|----------------------------------------------------
2  | Extras_x86_64  | CentOS 7 | yum          | http://mirror.centos.org/centos-7/7/extras/x86_64/
1  | OS_x86_64      | CentOS 7 | yum          | http://mirror.centos.org/centos-7/7/os/x86_64/
3  | Updates_x86_64 | CentOS 7 | yum          | http://mirror.centos.org/centos-7/7/updates/x86_64/
---|----------------|----------|--------------|----------------------------------------------------
root@katello#
..
t@katello# hammer product list  --organization "MSA"
---|----------|----------------|--------------|--------------|------------------
ID | NAME     | DESCRIPTION    | ORGANIZATION | REPOSITORIES | SYNC STATE
---|----------|----------------|--------------|--------------|------------------
1  | CentOS 7 | Sync Base Repo | MSA          | 3            | Syncing Complete.
---|----------|----------------|--------------|--------------|------------------
root@katello#
..
t@katello#  hammer lifecycle-environment list
---|----------------|--------
ID | NAME           | PRIOR
---|----------------|--------
2  | Library        |
1  | Library        |
3  | Non Production | Library
4  | Production     | Library
---|----------------|--------
root@katello#
t@katello# hammer content-view version list
---|-------------------------------|---------|------------------------
ID | NAME                          | VERSION | LIFECYCLE ENVIRONMENTS
---|-------------------------------|---------|------------------------
4  | CentOS content view 2.0       | 2.0     | Library, Non Production
3  | CentOS content view 1.0       | 1.0     | Production
2  | Default Organization View 1.0 | 1.0     | Library
1  | Default Organization View 1.0 | 1.0     | Library
---|-------------------------------|---------|------------------------
root@katello#
t@katello# hammer activation-key list --organization=MSA
---|-------------------|----------------|-----------------------|--------------------
ID | NAME              | HOST LIMIT     | LIFECYCLE ENVIRONMENT | CONTENT VIEW
---|-------------------|----------------|-----------------------|--------------------
1  | Centos_7_Prod_key | 1 of Unlimited | Production            | CentOS content view
---|-------------------|----------------|-----------------------|--------------------
root@katello#
..
t@katello# hammer subscription list --organization=MSA
---|----------------------------------|----------|----------|----------|---------|---------|---------------------|-----------|---------
ID | UUID                             | NAME     | TYPE     | CONTRACT | ACCOUNT | SUPPORT | END DATE            | QUANTITY  | CONSUMED
---|----------------------------------|----------|----------|----------|---------|---------|---------------------|-----------|---------
1  | 402808cd6618a286016618b4ecac0006 | CentOS 7 | Physical |          |         |         | 2048/09/19 01:46:41 | Unlimited | 1
---|----------------------------------|----------|----------|----------|---------|---------|---------------------|-----------|---------
..
<b>t@katello# hammer domain list</b>
---|------
ID | NAME
---|------
1  | bc.ca
---|------
root@katello#
t@katello# hammer proxy list
---|---------------|----------------------------|--------------------------
ID | NAME          | URL                        | FEATURES
---|---------------|----------------------------|--------------------------
1  | katello.bc.ca | https://katello.bc.ca:9090 | Pulp, TFTP, Puppet, Pu...
---|---------------|----------------------------|--------------------------
root@katello#
</pre>
