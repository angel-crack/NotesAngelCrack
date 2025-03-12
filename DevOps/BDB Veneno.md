[+] Check Envrioment Variables

```powershell hl:3
root@6d78561ae27c:/usr/local/lib/python3.11/site-packages/bdblib# env
SHELL=/bin/bash
BDBLIB_CONFIG=/code/AllProjects/taskers.json
COLORTERM=truecolor
TERM_PROGRAM_VERSION=1.95.3
HOSTNAME=6d78561ae27c
PYTHON_VERSION=3.11.2
SSH_AUTH_SOCK=/tmp/vscode-ssh-auth-66a7afd4-135f-4856-9a40-516afbec58ff.sock
REMOTE_CONTAINERS_IPC=/tmp/vscode-remote-containers-ipc-66a7afd4-135f-4856-9a40-516afbec58ff.sock
PWD=/usr/local/lib/python3.11/site-packages/bdblib
PYTHON_SETUPTOOLS_VERSION=65.5.1
HOME=/root
LANG=C.UTF-8
REMOTE_CONTAINERS=true
GPG_KEY=A035C8C19219BA821ECEA86B64E628F8D684696D
TERM=xterm-256color
REMOTE_CONTAINERS_SOCKETS=["/tmp/vscode-ssh-auth-66a7afd4-135f-4856-9a40-516afbec58ff.sock"]
SHLVL=2
PYTHON_PIP_VERSION=22.3.1
PYTHON_GET_PIP_SHA256=394be00f13fa1b9aaa47e911bdb59a09c3b2986472130f30aa0bfaf7f3980637
PYTHON_GET_PIP_URL=https://github.com/pypa/get-pip/raw/d5cb0afaf23b8520f1bbcfed521017b4a95f5c01/public/get-pip.py
BROWSER=/vscode/vscode-server/bin/linux-x64/f1a4fb101478ce6ec82fe9627c43efbf9e98c813/bin/helpers/browser.sh
PATH=/vscode/vscode-server/bin/linux-x64/f1a4fb101478ce6ec82fe9627c43efbf9e98c813/bin/remote-cli:/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
TERM_PROGRAM=vscode
VSCODE_IPC_HOOK_CLI=/tmp/vscode-ipc-97d8a353-00a2-4854-913d-3dd0c43ef97e.sock
_=/usr/bin/env
root@6d78561ae27c:/usr/local/lib/python3.11/site-packages/bdblib# 
```

`mkdir -p /path/to/non_existing_folder`

mkdir -p /BDBFILES/
touch /BDBFILES/BDBLIB_DBCONFIG.json


export BDBLIB_TOKEN="/BDBLIB/BDBLIB_TOKEN.json"
export BDBLIB_DBCONFIG="/BDBLIB/BDBLIB_DBCONFIG.json"

export BDBLIB_DBCONFIG="/path/to/your/file

BDBLIB_USER_ENVIROMENT="aubarnes"
BDBLIB_TOKEN="/BDBLIB/BDBLIB_TOKEN.json"
BDBLIB_DBCONFIG="/BDBLIB/BDBLIB_DBCONFIG.json"













1. Get BDBLib standard, we will get it using the official link
   
``` python
   pip download -i https://scripts-pypi.cisco.com/root/pypi/+simple -d ./offline_packages bdblib
```

2. Variables de Entorno necesarios:

BDBLIB_USER_ENVIROMENT ⇒ user name that runs the task, e.g. "aubarnes"
BDBLIB_TOKEN ⇒ Directory where BDBLIB_TOKEN.json will be located: Defined on Dockerfile, by default:
BDBLIB_DBCONFIG ⇒ Directory where BDBLIB_TOKEN.json will be located: Defined on Dockerfile, by default:
BDB_ALL_PROJECTS_PATH ⇒ Directory where all Projects are stored.

    "BDBLIB_USER_ENVIROMENT": "aubarnes",
    "BDBLIB_TOKEN": "/BDBFILES/BDBLIB_TOKEN.json",
    "BDBLIB_DBCONFIG": "/BDBFILES/BDBLIB_DBCONFIG.json"
    "BDBLIB_ALL_PROJECTS_PATH": /code/AllProjects/


