### PPDM Kubernetes with OpenShift – Install Roadmap

1. **Patch CNDM / controller properties for OADP**

   * Update **CNDM / controller configuration** to use the required **OADP version (for example, OADP stable 1.5.3)** as documented for your PPDM release.

2. **Apply PPDM RBAC resources**

   * Download the latest **RBAC archive (`rbac.tar.gz`)** from the PPDM UI:  
     `Settings → Downloads → Kubernetes → RBAC`.

   * Extract and apply the RBAC YAMLs (for example, `ppdm-controller-rbac.yaml` and `ppdm-discovery.yaml`) on the OpenShift cluster so the controller and discovery service accounts have the required permissions (including for InstallPlans / OADP).

3. **Create discovery service account and static token**

   * Create the **discovery service account** (if not already created by the RBAC YAMLs) following the `README.txt` from the RBAC tarball.

   * Create the **static token secret** for the discovery service account (as required by your Kubernetes/OpenShift version) and use this token in the **Kubernetes Host Credentials** configuration in PPDM.

4. **Enable Kubernetes asset source in PPDM**

   * In the PPDM UI, **enable the Kubernetes asset source**:  
     `Infrastructure → Asset Sources → Enable Source (Kubernetes)`.

5. **Add OpenShift cluster as Kubernetes asset source**

   * Add the **OpenShift cluster** as a **Kubernetes asset source** (API server FQDN/IP, port 6443, token, root CA as needed).

   * Verify that discovery completes and that **OpenShift projects (namespaces)** appear as Kubernetes assets.

6. **Onboard Kubernetes assets**

   * Validate the discovered **Kubernetes asset source** (OpenShift cluster) and confirm that required assets (namespaces and PVCs) are visible.

   * Run a small **test protection and restore** on a non-critical namespace to confirm end-to-end functionality before putting customer workloads under policy.
