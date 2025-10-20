# Note: Ansible Directory

The `ansible/` directory is a **separate local git repository** used by Semaphore UI.

## Why?

Semaphore requires a git repository to track playbooks. The ansible directory is:
- A local git repo (not pushed to GitHub)
- Mounted as a volume in Docker: `./ansible:/ansible`
- Used by Semaphore at path: `file:///ansible`

## Setup

The ansible directory is already initialized. If you need to recreate it:

```bash
cd ansible
git init
git add .
git commit -m "Ansible playbooks"
```

## GitHub Integration

To use your own GitHub repository with Semaphore instead of the local files:

1. Create a new GitHub repository for your playbooks
2. Push the ansible directory contents:
   ```bash
   cd ansible
   git remote add origin https://github.com/yourusername/ansible-playbooks.git
   git push -u origin main
   ```
3. In Semaphore UI, update Repository URL to:
   ```
   https://github.com/yourusername/ansible-playbooks.git
   ```

This allows you to manage playbooks in GitHub while testing them locally through the mounted volume.
