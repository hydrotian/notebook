---
layout: post
title: Use Claude Code and Gemini CLI for Vibe Coding on HPC
date: 2025-06-27 17:45:00 # Adjust date/time as needed
author: Tian # Or your name
comments: true
categories: [Programming, Unix, AI] # Example categories
---

It is crazy how fast Gen-AI for coding has been advancing recently, with game-changing tools coming out on a weekly basis. Thanks to that, my own workflow has evolved dramatically in the last few months. It feels like yesterday I was copying and pasting code from a ChatGPT window, and now I'm doing 'vibe coding' with CLI like Claude Code and Gemini CLI. Today, with some help from Gemini, I successfully installed these state-of-the-art tools on a HPC cluster, so that I don't need to install them on my local computer. Here's how I achieve this. It includes the initial Node.js installation, Gemini CLI and Claude Code installation.

## Part 1: Install a Compatible Node.js Version

Before installing Claude Code and Gemini using their official command lines, you have to install Node.js v18 or later. However, running the offical Node.js build failed as my HPC system's library is old: 

`node: /lib64/libc.so.6: version 'GLIBC_2.28' not found (required by node)`

The solution is to find a version of Node.js that was compiled for an older system from this [Node.js Unofficial Builds page](https://unofficial-builds.nodejs.org/). 

- First we need to find the GLIBC version for the HPC using `ldd --version`. My system returns `GLIBC 2.17`.
- Navigate to the Node.js Unofficial Builds page.
Click into the directory for the Node.js version you want (e.g., v20.15.1/).
- Find the file that matches your architecture (linux-x64) and your GLIBC version. For example, if your system has GLIBC 2.17, download the file named `node-v20.15.1-linux-x64-glibc-2.17.tar.gz` from [this page](https://unofficial-builds.nodejs.org/download/release/v20.15.1/).
- Use wget on your HPC to download it to your home directory.
- Use `tar -xf *.tar.gz` to untar the nodejs file and rename it to something simpler like `nodejs`.
- Add custom Node.js installation to PATH in the `~/.bashrc` file: `export PATH=$HOME/nodejs/bin:$PATH`.
- `source ~/.bashrc`

## Part 2: Install the Google Gemini CLI
- I had some issues with the installation command `npm install -g @google/gemini-cli`, so I used the command provided from [here](https://github.com/google-gemini/gemini-cli): `npx https://github.com/google-gemini/gemini-cli` and it worked.
- Then when you trying to start Gemini, there's a authentication process, usually a webpage will pop out and you sign in. However now we are working with and HPC, which didn't have smooth link to webpages.
- I did see google log in page opened on my local web browser, I signed in and grant permissions
- Here's a tricky part, your browser will redirect to a localhost URL and show an error. This is expected. Copy this entire localhost URL.
- Open another terminal **on the HPC** (while leaving the Gemini CLI running in the first one). Paste the full localhost URL you copied from your browser into the following command: `curl "http://localhost:XXXXX"`
- The terminal running gemini will now be authenticated.
- One more issue is that the `npx` command needs to be executed everytime log into the HPC. So I added a line to create an alias in the `~/.bashrc`: `alias gemini='npx https://github.com/google-gemini/gemini-cli'`. The good news is that you don't need to be authenticated again.

## Part 3: Install Claude Code
- Claude Code can be installed without error message using `npm install -g @anthropic-ai/claude-code`.
- However, when running it using `claude` command, it failed due to a shebang incompatibility as the old system on the HPC doesn't know the `-S` option. To solve this, Gemini helped me create a wrapper script in `$HOME/bin/claude-exec`:

```bash
#!/bin/bash
# This script acts as a modern replacement for 'env -S' on older systems.
# It correctly calls Node.js with multiple arguments before running the main script.

# 1. The full path to your custom Node.js installation
#    (You can verify this with 'which node')
NODE_EXEC_PATH="$HOME/nodejs/bin/node"

# 2. The full path to the *real* claude script
#    (You can verify this with 'which claude')
CLAUDE_SCRIPT_PATH="$HOME/nodejs/bin/claude"

# 3. Execute Node.js with the required options, the claude script,
#    and pass along any other arguments from the command line ("$@")
exec "$NODE_EXEC_PATH" --no-warnings --enable-source-maps "$CLAUDE_SCRIPT_PATH" "$@"

```
Then add an alias to `.bashrc` file: `alias claude='$HOME/bin/claude-exec'`.

- The authentication process is the same and since this time Claude Code was installed by `npm` (not `npx` for Gemini CLI), you don't need to install it again.

- There's a good [tool bar](https://github.com/leeguooooo/claude-code-usage-bar) to help you lively tracking how much you spent on Claude Code. I used [tmux](https://github.com/tmux/tmux/wiki) to show it at the bottom of my terminal.