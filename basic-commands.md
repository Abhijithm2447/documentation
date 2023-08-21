# basic commands

### Stop running process

```bash
docker stop $(docker ps -a -q)
```

### Delete containers

```bash
docker system prune -a
```
