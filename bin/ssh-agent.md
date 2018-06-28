## ssh-agent

ssh-agent is the default agent included with OpenSSH

    ssh-agent

```
SSH_AUTH_SOCK=/tmp/ssh-vEGjCM2147/agent.2147; export SSH_AUTH_SOCK;
SSH_AGENT_PID=2148; export SSH_AGENT_PID;
echo Agent pid 2148;
```
## use the ssh-agent variables

    eval $(ssh-agent)

