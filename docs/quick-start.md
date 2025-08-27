# Quick Start Guide

## Prerequisites Checklist

- [ ] Ansible 2.9+ installed
- [ ] Target Ubuntu/Debian server ready
- [ ] SSH key-based access configured
- [ ] GitHub Personal Access Token generated

## 5-Minute Setup

### 1. Clone and Configure

```bash
git clone https://github.com/Maede-alv/jenkins-master-ansible.git
cd jenkins-master-ansible

# Configure inventory
cp inventory/hosts.yml.example inventory/hosts.yml
# Edit inventory/hosts.yml with your server details

# Configure variables
cp vars/config.yml.example vars/config.yml
# Edit vars/config.yml with your credentials
```

### 2. Update Configuration

Edit `inventory/hosts.yml`:
```yaml
all:
  hosts:
    jenkins_master:
      ansible_host: "YOUR_SERVER_IP"
      ansible_user: "ubuntu"
      ansible_ssh_private_key_file: "/path/to/your/key.pem"
```

Edit `vars/config.yml`:
```yaml
jenkins_admin_username: "admin"
jenkins_admin_password: "your-secure-password"
github_username: "your-github-username"
github_token: "your-github-token"
jobdsl_repo_url: "https://github.com/Maede-alv/jenkins-jobdsl.git"
```

### 3. Deploy

```bash
ansible-playbook -i inventory/hosts.yml playbook.yml
```

### 4. Access Jenkins

- **URL**: `http://YOUR_SERVER_IP:8080`
- **Username**: `admin`
- **Password**: Your configured password

### 5. Verify Setup

1. Login to Jenkins
2. Check that the "seed-job" exists
3. Run the seed job to load your DSL scripts
4. Verify your pipeline jobs are created

## Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| SSH connection fails | Check key permissions (600) and server connectivity |
| Jenkins won't start | Verify Java installation and port availability |
| Seed job fails | Check GitHub token permissions and repository access |
| Plugins not installing | Ensure internet connectivity and valid plugin versions |

## Next Steps

1. **Add Agents**: Use [jenkins-agent-ansible](https://github.com/Maede-alv/jenkins-agent-ansible)
2. **Customize Jobs**: Modify [jenkins-jobdsl](https://github.com/Maede-alv/jenkins-jobdsl) repository
3. **Security**: Set up HTTPS and firewall rules
4. **Monitoring**: Configure logging and monitoring

## Support

- **Documentation**: See main [README.md](../README.md)
- **Architecture**: Check [architecture-diagram.md](architecture-diagram.md)
- **Issues**: Report on GitHub repository 