# AAP 2.5 Single Growth Node Installation

⚠️ **IMPORTANT: This project is currently untested and under development. Use at your own risk in non-production environments only.**

This Ansible project provides a complete workflow to download, prepare, and install Red Hat Ansible Automation Platform (AAP) 2.5 on a single growth node using containerized deployment.

## Project Structure

```bash
roles/
├── aap_setup_download/     # Downloads AAP installer tarball
├── aap_setup_prepare/      # Prepares installation and SSL certificates
└── aap_setup_install/      # Executes the AAP installation
```

## Requirements

- Red Hat Enterprise Linux 8 or 9
- Root access or sudo privileges
- Valid Red Hat subscription with AAP entitlements
- Red Hat Registry credentials
- Red Hat API offline token (from <https://access.redhat.com/management/api/>)

## Required Variables

```yaml
# Red Hat API access
aap_setup_down_offline_token: "your_offline_token"

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

## Optional Variables

```yaml
# SSL Certificates (default: false)
use_custom_certs: true

# Download configuration
aap_setup_down_version: "2.5"
aap_setup_down_type: "containerized-setup"
aap_setup_rhel_version: 8

# Installation control
aap_setup_inst_force: false
```

## SSL Certificate Management

To use custom SSL certificates, place these files in `roles/aap_setup_prepare/files/`:

- `aap.crt` - SSL certificate
- `aap.key` - SSL private key  
- `ca-bundle.crt` - Certificate Authority bundle

Set `use_custom_certs: true` to enable.

## Usage

Run the complete installation playbook:

```bash
ansible-playbook -i inventory playbook.yml
```

## Architecture

Single growth node configuration with:

- Automation Controller (hybrid node)
- Automation Hub
- Event-Driven Ansible
- PostgreSQL Database (containerized)

## Dependencies

This project wraps the following collection roles:

- `infra.aap_utilities.aap_setup_download`
- `infra.aap_utilities.aap_setup_prepare`
- `infra.aap_utilities.aap_setup_install`

Install the collection:

```bash
ansible-galaxy collection install infra.aap_utilities
```

## Post-Installation Access

- **Controller**: `https://<hostname>/`
- **Hub**: `https://<hostname>/hub/`
- **EDA**: `https://<hostname>/eda/`

## Testing Status

⚠️ **This project has not been fully tested yet. Please:**

- Test in development environments first
- Report issues via GitHub
- Contribute improvements and bug fixes
- Validate in your specific environment before production use

## Contributing

This project is actively being developed. Contributions, testing feedback, and bug reports are welcome.

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
