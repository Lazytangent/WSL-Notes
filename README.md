# WSL Notes

Just a compilation of notes for someone debugging WSL (Windows Subsystem Linux)
errors.

These particular methods were tried on WSL2 with Ubuntu.

## Installing WSL

For best results, use the instructions that Microsoft documents
[here](https://docs.microsoft.com/en-us/windows/wsl/install-win10). Usually, we
would just use the Manual install instructions, unless you meet the requirements
to use the Simplified install instructions.

### Setting up WSL

Install whatever other applications you need on WSL based on Microsoft's
documentation:

* [VSCode](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode)
* [Git](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-git)
* [Databases](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database)
* [Docker](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers)

## Debugging Issues

From here, we'll go over some common errors you'll see with web development on
WSL.

### Localhost not connecting

Occasionally, servers running in WSL will not connect to Windows native
`http://localhost` address. Options to fix this problem include:

1. Finding the actual IP address WSL is running on to connect to that IP in your
   browser (or wherever) to have access to the WSL-run servers.

You can run the following command to find the IP address from within your WSL
environment:

```sh
ip addr | grep eth0
```

2. Shutting down the current running WSL environment to reset the connection to
   Windows's localhost.

In Powershell:

1. Run `wsl --shutdown`
2. Open a new instance of WSL
3. Restart your database
    * For PostgreSQL on Ubuntu, run `sudo service postgresql start`
4. Start your server and try to connect from Windows's localhost

You can optionally just restart your PC to restart WSL.

### Kill Run-Away Node Servers

Use the following command to kill any server that was left running when you
closed out of a terminal that was running it.

```sh
sudo kill -9 $(sudo lsof -t -i:«port_number»)
```

### `apt-get update` Release File not yet valid

Sometimes the clock in Ubuntu in WSL will drift away from the clock of your PC.
You can fix that by running:

```sh
sudo hwclock --hctosys
```

### Postgres Issues

If running `psql` and `psql -h localhost` give you slightly different prompts,
there's probably two instances of Postgres running on the computer: one in WSL
and one in Windows. Odds are, your application will try to connect to the
Windows instance of Postgres. To avoid this issue, you can add the following
alias to the bottom of your `rc` file (`.bashrc` or `.zshrc` depending on which
shell you're running).

```sh
alias psql="psql -h localhost"
```
