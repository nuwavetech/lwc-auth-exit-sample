# LightWave Client

## Sample Auth Exit Token Server

This repository contains a sample Auth Exit Token Server for LightWave Client. Its purpose is to demonstrate the basic interaction between LightWave Client and an Auth Exit token server.

Although the program is written in C and runs under the Guardian operating system, the specific Auth Exit functionality is straightforward enough to port to a C or Java program running under Open System Services. The only requirements are that the program be able to run in a Pathway Serverclass and to provide one or more tokens to a requestor like LightWave Client.

## Sample Contents

| Repository file | NonStop filename | Description |
| --- | --- | --- |
| ./resources |  | Miscellaneous files |
| bldcli.tacl | bldcli | A TACL script for compiling ctknsrv.c. |
| clipwy.txt | clipwy | A Pathway serverclass configuration for the ctknsrv object as a serverclass. |
| ./src |  |  |
| ctknsrv.c | ctknsrvc | The NonStop C source code for the sample token server program. |
| README.md |  | This file |

## Building lwae.h

Compilation of the sample token server program requires the LightWave Auth Exits header file, ```lwae.h```. The DDL source from which the header file is produced is included with the LightWave Client distribution, ```LWAEDDL```.

``` tacl
tacl> volume aesample
tacl> ddl2
!?dictn, nosave
!?c lwaeh !
?source <lwc-distro-svol>.lwaeddl
?exit
```

Where ```<lwc-distro-svol>``` is the subvolume where the ```LWAEDDL``` file is stored.

DDL should compile ```LWAEDDL``` with no errors. ```lwaeh``` is then available for the ```ctknsrvc``` compilation.

## Building the Token Server

After uploading the repository contents to the NonStop and compiling the ```LWAEDDL``` source to the ```lwae.h``` header file, the sample token server can be compiled.

``` tacl
tacl> volume aesample
tacl> run bldcli
```

This step compiles the ```ctknsrv.c``` source to the ctknsrv program.

## Adding the Token Server to Pathway

LightWave Client communicates with the Token Server via Pathsend calls to the token server's serverclass.

A Pathway serverclass configuration for the sample ctknsrv program is included in this repository:

``` pathcom
reset server
set server cpus 0:1
set server createdelay 0 secs
set server deletedelay 20 secs
set server highpin on
set server linkdepth 1
set server maxservers 20
set server maxlinks 4
set server numstatic 0
set server program ctknsrv
set server tmf on
set server debug off
set server in $zhome
set server out $zhome
add server lwc-tkn-svr
```

The name given to the serverclass, in this case ```lwc-tkn-svr```, must be the same name specified in the LWC Auth Exit configuration file identified by the ```--auth-exit``` command-line option.

For further details on Auth Exit configuration, IPMs, and functionality, visit the online NuWave Documentation Center: https://docs.nuwavetech.com/.
