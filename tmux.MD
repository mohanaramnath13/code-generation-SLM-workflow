Tmux (short for Terminal Multiplexer) is an essential tool for working on remote servers like the one running your LiveCodeBench evaluation. It allows you to run long jobs and keep them active even if your SSH connection breaks or you close your laptop.

Tmux commands:
## A. Session Management (Start, Rejoin, Kill)

These are the commands you use directly in your shell (outside of a Tmux session):

| Goal               | Command                        | Description |
|-------------------|--------------------------------|-------------|
| **Start a New Session** | `tmux new -s my_lcb_run`        | Creates a new session named `my_lcb_run` and attaches you to it. |
| **List All Sessions**   | `tmux ls`                       | Shows all currently running sessions on the server. |
| **Rejoin a Session**    | `tmux attach -t my_lcb_run`     | Reconnects you to the named session. Use this after a “Broken pipe” error. |
| **Kill a Session**      | `tmux kill-session -t my_lcb_run` | Forcefully closes a running session (use only when necessary). |

## B. Inside a Tmux Session (Using `Ctrl+B`)

Once you are inside the session, these are the commands you need.  
Remember to press and release **Ctrl+B** first.

| Command Key | Full Command     | Goal |
|-------------|------------------|------|
| **D**       | `Ctrl+B, D`      | **Detach** from the current session. The session continues running in the background. |
| **C**       | `Ctrl+B, C`      | Create a **new Window** inside the current session (like opening a new tab). |
| **N**       | `Ctrl+B, N`      | Move to the **Next** window. |
| **P**       | `Ctrl+B, P`      | Move to the **Previous** window. |
| **%**       | `Ctrl+B, %`      | Split the current window **Vertically** (useful for monitoring logs). |
| **"**       | `Ctrl+B, "`      | Split the current window **Horizontally**. |


