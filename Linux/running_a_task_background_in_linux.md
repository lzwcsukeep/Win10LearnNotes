## Run a Linux Command in the Background [#](https://linuxize.com/post/how-to-run-linux-commands-in-background/#run-a-linux-command-in-the-background)

To run a command in the background, add the ampersand symbol (`&`) at the end of the command:

在命令后面加一个'&' 符号，就可以将命令放在后台运行。

```shell
command &
```

The background process will continue to write messages to the terminal from which you invoked the command. To suppress the `stdout` and `stderr` messages use the following syntax:

后台任务运行时的输出仍然会到屏幕上，使用下面命令重定向输出到/dev/null

```shell
command > /dev/null 2>&1 & 
```

`>/dev/null 2>&1` means redirect `stdout` to `/dev/null` and [`stderr` to `stdout`](https://linuxize.com/post/bash-redirect-stderr-stdout/) .

附：

## Move a Foreground Process to Background

To move a running foreground process in the background:

1. Stop the process by typing `Ctrl+Z`.
2. Move the stopped process to the background by typing `bg`.

## Keep Background Processes Running After a Shell Exits [#](https://linuxize.com/post/how-to-run-linux-commands-in-background/#keep-background-processes-running-after-a-shell-exits)

If your connection drops or you log out of the shell session, the background processes are terminated. There are several ways to keep the process running after the interactive shell session ends.

### use `nohup`

The [`nohup`](https://linuxize.com/post/linux-nohup-command/) command executes another program specified as its argument and ignores all `SIGHUP` (hangup) signals. `SIGHUP` is a signal that is sent to a process when its controlling terminal is closed.

To run a command in the background using the `nohup` command, type:

```shell
nohup command &
```

The command output is redirected to the `nohup.out` file.

```shell
nohup: ignoring input and appending output to 'nohup.out'
```

If you log out or close the terminal, the process is not terminated.
