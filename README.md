# Semaphore UI with Ansible Playbooks

Docker Compose setup for Semaphore UI with ready-to-use Ansible playbooks for macOS testing and GitHub integration.

## 🚀 Quick Start

### 1. Start Semaphore UI

```bash
docker compose up -d
```

### 2. Access Semaphore

- **URL**: http://localhost:3001
- **Username**: `admin`
- **Password**: `changeme`

⚠️ **Change the password after first login!**

### 3. Configure Repository in Semaphore UI

1. Go to **Repositories** → **New Repository**
2. Fill in:
   - **Name**: `Ansible Playbooks`
   - **URL**: `file:///ansible`
   - **Branch**: `main`
   - **Access Key**: None
3. Click **Create**

### 4. Create Inventory

1. Go to **Inventory** → **New Inventory**
2. Fill in:
   - **Name**: `Local macOS`
   - **Type**: `Static`
   - **Inventory**:
     ```ini
     [local]
     localhost ansible_connection=local ansible_python_interpreter=/usr/bin/python3
     ```
3. Click **Create**

### 5. Create Task Template

1. Go to **Task Templates** → **New Template**
2. Fill in:
   - **Name**: `macOS System Test`
   - **Playbook**: `playbooks/test_macos.yml`
   - **Inventory**: Select `Local macOS`
   - **Repository**: Select `Ansible Playbooks`
3. Click **Create**

### 6. Run Your Task

1. Go to **Tasks** → **New Task**
2. Select your template
3. Click **Run**
4. Watch it execute! 🎉

## 📋 Available Playbooks

### `test_macos.yml` - System Test
Tests Ansible on macOS with system checks, file operations, and diagnostics.

```bash
cd ansible
ansible-playbook playbooks/test_macos.yml
```

### `github_integration.yml` - GitHub Repository Management
Clones or updates a GitHub repository.

```bash
ansible-playbook playbooks/github_integration.yml
```

**Custom repository:**
```bash
ansible-playbook playbooks/github_integration.yml \
  -e "github_repo_url=https://github.com/yourusername/yourrepo.git" \
  -e "repo_destination=/tmp/myrepo"
```

### `workflow.yml` - Complete Workflow
Combined test and GitHub integration with reporting.

```bash
ansible-playbook playbooks/workflow.yml
```

## 📁 Project Structure

```
.
├── docker-compose.yml      # Semaphore UI container
├── README.md              # This file
└── ansible/
    ├── ansible.cfg        # Ansible configuration
    ├── README.md         # Ansible documentation
    ├── inventory/
    │   └── hosts         # Localhost inventory
    ├── playbooks/
    │   ├── test_macos.yml           # System test
    │   ├── github_integration.yml   # GitHub operations
    │   └── workflow.yml             # Combined workflow
    └── vars/
        └── github.yml    # GitHub variables
```

## 🔧 Configuration

### Docker Compose
- **Port**: 3001:3000
- **Database**: BoltDB (embedded)
- **Volume**: `semaphore-data` (persistent storage)
- **Ansible Mount**: `./ansible:/ansible` (read-only)

### Ansible
- **Connection**: Local (no SSH)
- **Python**: `/usr/bin/python3`
- **Inventory**: `inventory/hosts`
- **Output**: YAML format

## 🐳 Docker Commands

```bash
# Start
docker compose up -d

# Stop
docker compose down

# View logs
docker compose logs -f

# Restart
docker compose restart

# Reset (removes all data)
docker compose down -v
```

## 🧪 Test Locally

```bash
cd ansible

# Test system
ansible-playbook playbooks/test_macos.yml

# Test GitHub integration
ansible-playbook playbooks/github_integration.yml

# Run workflow
ansible-playbook playbooks/workflow.yml
```

## 🛠 Troubleshooting

### Can't login?
Reset Semaphore:
```bash
docker compose down -v
docker compose up -d
```
Then login with admin/changeme

### Repository not found?
- Verify URL is `file:///ansible` (3 slashes)
- Verify branch is `main`
- Restart: `docker compose restart`

### Playbook not found?
- Playbook path should be `playbooks/test_macos.yml` (no leading `/`)
- Check files exist: `docker exec semaphore ls -la /ansible/playbooks`

### Python not found?
Update inventory to:
```ini
localhost ansible_connection=local ansible_python_interpreter=/opt/homebrew/bin/python3
```

## 📚 Learn More

- **Ansible**: https://docs.ansible.com/
- **Semaphore UI**: https://docs.semaphoreui.com/
- **Docker Compose**: https://docs.docker.com/compose/

## 📝 Prerequisites

- Docker and Docker Compose
- Ansible (for local testing): `brew install ansible`

## 🎯 Next Steps

1. ✅ Change admin password
2. 📅 Create schedules for automated runs
3. 🔔 Set up notifications (Slack, email)
4. 🎨 Customize playbooks for your needs
5. 🔐 Use Ansible Vault for sensitive data

---

**Made with ❤️ for automation**
