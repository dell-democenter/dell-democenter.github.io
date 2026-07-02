# ppdm-cli start options 

ppdm-cli is opre-installed an configured from launchpad.

however, the more elegant otion is starting a containerized instance from the ansible host ( authors choice )

## ppdm-cli from ansible

once ansible host s started, ssh into from remoteNG

run the following:

```bash
https://api.openai-compat.model-serving.eu01.onstackit.cloud/v1
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


