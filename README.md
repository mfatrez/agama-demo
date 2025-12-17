# Agama Demo

Automatic installation using an automated Agama profile

## Demo prep

### Agama profile repository

run a http server on port 8080 serving sles16.json

```
python3 -m http.server 8080
```

### Installed server

Boot from CD, select "Install..." and edit with (e). Add this snippet below to the end of line "linux ($root) ..."

```
ip=dhcp inst.auto=http://192.168.0.15:8080/sles16.json
```

After installation, test cockpit url for [sles16-001](https://sles16-001:9090/)

## Structure Agama Profile

```
{
  "product": {},               # Identifies the OS/product to be installed.
  "root": {},                  # Root credentials for administrative access.
  "user": {},                  # First non-root user account.
  "localization": {},          # Language, keyboard, and time zone settings.
  "hostname": {},              # Static/transient hostname settings.
  "software": {},              # Packages and patterns to be installed.
  "storage": {},               # Partitioning and mount configuration.
  "bootloader": {},            # Boot loader config and kernel params.
  "network": {},               # Network interface configuration.
  "security": {},              # SSL certs and other security settings.
  "scripts": {},               # Pre/post install scripting.
  "files": {},                 # Inject additional files post-install.
  "legacyAutoyastStorage": {}, # Support for legacy AutoYaST JSON-style storage.
  "iscsi": {},                 # iSCSI disk/target configuration.
  "dasd": {}                   # DASD disk support for IBM Z (s390x).
}
```


## Demo example : sles-001.json

```
{
  "product": {
    "id": "SLES",
    "registrationCode": "XXXXXXXXXXXXXXXXXXXXXXXXXXX",
    "registrationEmail": "XXXXXXXXXXXXXXXXXXXXXXXXXXX"
  },
  "hostname": {
    "transient": "sles16-001"
  },
  "root": {
    "hashedPassword": true,
    "password": "$6$zTVOTgBGAKOG2VLM$rFlb....",
    "sshPublicKey": "ssh-rsa  AAAAC3Nz..."
  },
  "user": {
    "hashedPassword": false,
    "fullName": "User",
    "userName": "user",
    "password": "passwd"
  },
  "localization": {
    "language": "en_US.UTF-8",
    "keyboard": "fr",
    "timezone": "Europe/Paris"
  },
  "software": {
    "patterns": [
      "minimal_base",
      "cockpit",
      "selinux"
    ],
    "packages": [
       "vim",
       "htop",
       "curl"
    ]
  },
  "network": {
    "connections": [
      {
        "id": "Wired Connection",
        "method4": "manual",
        "gateway4": "192.168.0.1",
        "method6": "auto",
        "addresses": [
          "192.168.0.16/24"
        ],
        "nameservers": [
          "192.168.0.1"
        ],
        "ignoreAutoDns": false,
        "status": "up",
        "autoconnect": true,
        "persistent": true
      }
    ]
  }
}
```