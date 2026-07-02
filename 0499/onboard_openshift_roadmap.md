# PPDM Kubernetes with OpenShift – Install Roadmap

all oc command are run from the ansible host 
log in as admin and cd to ~/workspace/0499/lab3

## automatic onboarding

just run the automated ansible playbook.
Please make sure to checkout the correct git branch if you update you ppdm to a newer version

```bash
ansible-playbook ~/workspace/ansible_ppdm/130.1_playbook_rbac_add_k8s_to_ppdm.yaml
```


## Manual Onboarding
1. **Validate required OADP version/ Supported OCP Version**

   **CNDM will update the OADP Version automatically and patch install plans accordingly**
  
     
   | OCP Version | OADP Version | OADP Channel |
   |-------------|--------------|--------------|
   | 4.14        | 1.3.0        | stable-1.3   |
   | 4.15–4.18*  | 1.4.3        | stable-1.4   |
   | 4.19-4.21   | 1.5.x        | stable       |

2. **Apply PPDM RBAC resources and Create discovery service account  static token**

   * Download the latest **RBAC archive (`rbac.tar.gz`)** from the PPDM UI:  
     `Settings → Downloads → Kubernetes → RBAC`.

   * Extract and apply the RBAC YAMLs (for example, `ppdm-controller-rbac.yaml` and `ppdm-discovery.yaml`) on the OpenShift cluster so the controller and discovery service accounts have the required permissions (including for InstallPlans / OADP),  following the `README.txt` from the RBAC tarball.

   * Create the **static token ** for the discovery service account  and use this token in the **Kubernetes Host Credentials** configuration in PPDM.

3. **Enable Kubernetes asset source in PPDM**

   * In the PPDM UI, **enable the Kubernetes asset source**:  
     `Infrastructure → Asset Sources → Enable Source (Kubernetes)`.

4. **Add OpenShift cluster as Kubernetes asset source**

   * Add the **OpenShift cluster** as a **Kubernetes asset source** (API server FQDN/IP, port 6443, token, root CA as needed).

   * Verify that discovery completes and that **OpenShift projects (namespaces)** appear as Kubernetes assets.

5. **Onboard Kubernetes assets**

   * Validate the discovered **Kubernetes asset source** (OpenShift cluster) and confirm that required assets (namespaces and PVCs) are visible.

   * Run a small **test protection and restore** on a non-critical namespace to confirm end-to-end functionality before putting customer workloads under policy.
