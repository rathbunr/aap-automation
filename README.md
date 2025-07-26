# AAP Prepare Role

This Ansible role prepares and installs Red Hat Ansible Automation Platform (AAP) 2.5 on a single growth node using containerized deployment.

## Requirements

- Red Hat Enterprise Linux 8 or 9
- Root access or sudo privileges
- Valid Red Hat subscription with AAP entitlements
- Red Hat Registry credentials

## Role Variables

### Required Variables

```yaml
# AAP Component Admin Passwords
aap_admin_password: "your_controller_admin_password"
aap_hub_admin_password: "your_hub_admin_password"
aap_eda_admin_password: "your_eda_admin_password"

# Database Passwords
aap_pg_password: "your_awx_db_password"
aap_hub_pg_password: "your_hub_db_password"
aap_eda_pg_password: "your_eda_db_password"

# Red Hat Registry Credentials
aap_registry_username: "your_redhat_username"
aap_registry_password: "your_redhat_password"

# Hub URL
aap_hub_url: "https://hub.example.com"
```

### Optional Variables

```yaml
# SSL Certificates (default: false)
use_custom_certs: true

# Skip installer download if already present (default: true)
aap_setup_download_enabled: false
```

## SSL Certificate Management

To use custom SSL certificates, place these files in `roles/aap_prepare/files/`:

- `aap.crt` - SSL certificate
- `aap.key` - SSL private key  
- `ca-bundle.crt` - Certificate Authority bundle

Set `use_custom_certs: true` to enable.

## Usage

```yaml
---
- name: Install AAP 2.5
  hosts: all
  become: true
  roles:
    - aap_prepare
```

## Architecture

Single growth node with all AAP components:

- Automation Controller (hybrid node)
- Automation Hub
- Event-Driven Ansible
- PostgreSQL Database

## Dependencies

- `infra.aap_utilities.aap_setup_download`
- `infra.aap_utilities.aap_setup_prepare`
- `infra.aap_utilities.aap_setup_install`

## Post-Installation Access

- **Controller**: `https://<hostname>/`
- **Hub**: `https://<hostname>/hub/`
- **EDA**: `https://<hostname>/eda/`
---
- name: Install AAP 2.5
  hosts: all
  become: true
  roles:
    - aap_prepare
```

### Example Extra Variables

```yaml
---
aap_admin_password: "MySecurePassword123!"
aap_hub_admin_password: "MyHubPassword123!"
aap_eda_admin_password: "MyEdaPassword123!"
aap_pg_password: "MyDbPassword123!"
aap_hub_pg_password: "MyHubDbPassword123!"
aap_eda_pg_password: "MyEdaDbPassword123!"
aap_registry_username: "myuser@redhat.com"
aap_registry_password: "myregistrypassword"
aap_hub_url: "https://hub.mycompany.com"
use_custom_certs: false
```

## Architecture

This role configures a single growth node with:

- **Automation Controller**: Web UI and API for job orchestration
- **Automation Hub**: Private content repository
- **Event-Driven Ansible**: Event-driven automation
- **PostgreSQL Database**: Shared database for all components

## Dependencies

- `infra.aap_utilities.aap_setup_download` - Downloads AAP installer
- `infra.aap_utilities.aap_setup_prepare` - Prepares installation
- `infra.aap_utilities.aap_setup_install` - Runs installation

## Post-Installation

After successful installation, access your AAP services at:

- **Controller**: `https://<hostname>/`
- **Hub**: `https://<hostname>/hub/`
- **EDA**: `https://<hostname>/eda/`

Default admin credentials use the passwords specified in your variables.

## Troubleshooting

- Ensure all required variables are defined
- Verify Red Hat subscription and registry access
- Check system requirements (CPU, memory, disk space)
- Review installation logs in the AAP working directory

## License

GPL-3.0

## Author

Created for AAP 2.5 single growth node deployments.
