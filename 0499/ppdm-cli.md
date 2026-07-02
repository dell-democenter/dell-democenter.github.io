# ppdm-cli start options 

ppdm-cli is opre-installed an configured from launchpad.

however, the more elegant otion is starting a containerized instance from the ansible host ( authors choice )

## ppdm-cli from ansible

once ansible host s started, ssh into from remoteNG

run the following:

*on ansible host*
```bash
podman run -it --rm --pull=always  -v /home/admin/data01:/data01 \
  -e PPDM_SERVER=ppdm-1.demo.local \
  -e PPDM_USERNAME=admin \
  -e PPDM_PASSWORD=Password123! \
  -e PPDM_PORT=8443 -e PPDM_INSECURE_SKIP_VERIFY=true \
  quay.io/delldps/ppdm-cli:20.1.0.0-amd64
```

this starts latest ppdm-cli vi podman, exposes port 8080 on ansible host for web-monitor, and mounts /data01

## Authors favourite commands

### Upgrade "workflow"

the ppdm-cl upgrade workflow upgarde a ppdm instance.
Download you update package to /home/admin/data01 on ansible host

pro tip: mount the nas share to data01, usde a new ansinle host session ( will be done autmatically in future version )

*on ansible host*
```bash
sudo mount -t nfs nas.demo.local:/datastore/shared /home/admin/data01
```

now, form the ppdm-cli container on the ansible host, start the upgrade workflow

*from ppdm-cli container*
```bash
ppdm-cli upgrade workflow --continuous --automatic --force --file /data01/artifacts/ppdm/dellemc-ppdm-upgrade-sw-20.2.0.0-14.pkg
```


watch the beauy :-)
<img width="1357" height="548" alt="image" src="https://github.com/user-attachments/assets/78517bf5-f9e3-41c4-a9ee-19d7d8a35c4a" />

