# Ansible 使用笔记

配置管理 / 批量部署工具。本目录提供最小可跑的 inventory、项目同步 playbook，以及常用命令速查。

## 目录

```text
ansible/
├── inventory.ini      # 主机清单示例
├── sync_project.yml   # 将本地项目 rsync 到远程
└── README.md          # 本说明
```

## 安装

```bash
# macOS
brew install ansible

# Ubuntu / Debian
sudo apt update && sudo apt install -y ansible

# 或用 pip（推荐隔离环境）
pip install ansible

# 同步模块依赖（sync_project.yml 用到）
ansible-galaxy collection install ansible.posix
```

验证：

```bash
ansible --version
ansible all -i inventory.ini -m ping
```

## SSH 免密登录（推荐）

Ansible 默认走 SSH，建议用密钥登录，避免在 inventory 里写密码。

```bash
# 生成专用密钥（注释可写成 ansible，方便区分）
ssh-keygen -t ed25519 -C "ansible"

# 将公钥拷到目标机（把 IP 换成实际地址）
ssh-copy-id -i ~/.ssh/id_ed25519.pub root@xxx.xxx.xxx.xxx
```

说明：

- 默认密钥路径为 `~/.ssh/id_ed25519`（私钥）和 `~/.ssh/id_ed25519.pub`（公钥）。
- 多台机器重复执行 `ssh-copy-id` 即可。
- 若密钥不叫默认名，inventory 里加：`ansible_ssh_private_key_file=~/.ssh/你的私钥`。
- 拷贝完成后可测：`ssh root@xxx.xxx.xxx.xxx`，应无需输入密码。

## 快速开始

### 1. 编辑 inventory

打开 `inventory.ini`，取消注释并填入真实 IP / 主机名，确认 `ansible_user` 与密钥路径。

### 2. 测试连通性

```bash
# 对某一组
ansible dev -i inventory.ini -m ping

# 对全部
ansible all -i inventory.ini -m ping

# 指定密钥
ansible all -i inventory.ini -m ping --private-key ~/.ssh/id_ed25519
```

### 3. 同步项目到远程

```bash
# 默认同步当前目录到 webservers 组的 /opt/app
ansible-playbook -i inventory.ini sync_project.yml

# 指定组、源目录、目标路径
ansible-playbook -i inventory.ini sync_project.yml \
  -e "target_hosts=dev project_src=./myapp remote_dest=/opt/myapp"

# 预览（不实际改远程）
ansible-playbook -i inventory.ini sync_project.yml --check --diff

# 仅某一台
ansible-playbook -i inventory.ini sync_project.yml --limit prod-app-01
```

## 常用命令

```bash
# 执行 ad-hoc 命令
ansible webservers -i inventory.ini -a "uptime"
ansible webservers -i inventory.ini -a "df -h" -b   # -b 提权

# 使用模块
ansible webservers -i inventory.ini -m copy -a "src=./app.conf dest=/etc/app.conf"
ansible webservers -i inventory.ini -m service -a "name=nginx state=restarted" -b

# playbook
ansible-playbook -i inventory.ini playbook.yml
ansible-playbook -i inventory.ini playbook.yml --tags deploy
ansible-playbook -i inventory.ini playbook.yml --skip-tags slow
ansible-playbook -i inventory.ini playbook.yml -v   # -vv / -vvv 更详细

# 列出主机
ansible-inventory -i inventory.ini --list
ansible-inventory -i inventory.ini --graph
```

## Inventory 要点

| 写法 | 说明 |
|------|------|
| `[group]` | 主机组 |
| `host ansible_host=IP` | 主机别名与实际地址 |
| `[group:children]` | 组包含子组 |
| `[all:vars]` / `[group:vars]` | 全局或组变量 |
| `ansible_connection=local` | 本机执行，不走 SSH |

YAML 格式 inventory 也可用，适合复杂变量；简单场景用 ini 即可。

## Playbook 结构速记

```yaml
- name: Example
  hosts: webservers
  become: true          # 需要 root 时
  vars:
    app_port: 8080
  tasks:
    - name: Install package
      ansible.builtin.apt:
        name: nginx
        state: present
    - name: Start service
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true
```

## 安全建议

- 生产环境用 **SSH 密钥**，不要在 inventory 里写明文密码。
- 敏感变量用 **Ansible Vault**：

```bash
ansible-vault create secrets.yml
ansible-vault edit secrets.yml
ansible-playbook -i inventory.ini playbook.yml --ask-vault-pass
# 或
ansible-playbook -i inventory.ini playbook.yml --vault-password-file ~/.vault_pass
```

- `sync_project.yml` 默认 `delete_extra: false`，避免误删远程文件；确认无误后再设为 `true`。
- 首次连接可用 `--check` 做 dry-run。

## 参考

- [Ansible 官方文档](https://docs.ansible.com/)
- [ansible.posix.synchronize](https://docs.ansible.com/ansible/latest/collections/ansible/posix/synchronize_module.html)
