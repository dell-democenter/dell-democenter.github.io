### PPDM Kubernetes with OpenShift – Install Roadmap

1. **Validate required OADP version from Install Guide and Patch CNDM if required**

   * Update **CNDM / controller configuration** to use the required **OADP version (for example, OADP stable 1.5.3)** as documented for your PPDM release
  
     
   | OCP Version | OADP Version | OADP Channel |
   |-------------|--------------|--------------|
   | 4.14        | 1.3.0        | stable-1.3   |
   | 4.15–4.18*  | 1.4.3        | stable-1.4   |
   | 4.19        | 1.5.x        | stable       |

**Note:** OCP 4.15–4.18 is supported by default; no additional configuration is needed.
**Note:** OCP 4.15–4.18 is supported by default; no additional configuration is needed.

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
