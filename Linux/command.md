# 🚀 Linux Commands

**🌐 A list of my commonly used Linux commands 🌐**

### Disk & Memory

| Command                                          | Description                              |
| ------------------------------------------------ | ---------------------------------------- |
| `du -sh * \| sort -h`                            | Check size at current pwd                |
| `sudo du -sh /\* 2>/dev/null \| sort -rh`        | Check size disk                          |
| `sudo du -sh ~/.cache 2>/dev/null`               | Check size home directory                |
| `sudo du -sh /var/\* 2>/dev/null \| sort -h`     | Check the size of subdirectories in /var |
| `sudo du -sh /var/log/\* 2>/dev/null \| sort -h` | Check log files                          |
| `sudo truncate -s 0 /var/log/large-log-file.log` | Clear specific log files                 |
| `sudo apt-get autoremove -y`                     | Remove unused packages                   |
| `sudo rm -rf /tmp/*`                             | Remove temporary files                   |
| `rm -rf ~/.cache/*`                              | Remove all cache files                   |

### CPU

| `cat /proc/cpuinfo` | Check CPU information |

### File

| `tail -f /var/log/nginx/access.log` | Monitor file content |

### Nginx Server

| `sudo nginx -v` | Check nginx version |
