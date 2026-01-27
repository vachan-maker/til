# Check if port is in use

To check which ports are in use and find which application is listening to which port. Use the following command:

```bash
sudo ss -tulpn | grep LISTEN
```

The command options are:

- `-t`:TCP Ports
- `-u`: UDP Ports
- `-l`: listening sockets
- `-p`: process using the port


[Source](https://www.cyberciti.biz/faq/unix-linux-check-if-port-is-in-use-command/)
