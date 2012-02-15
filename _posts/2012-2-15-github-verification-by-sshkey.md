---
layout: post
title: Github Push免输入密码
---

{{page.title}}
==============

<p class="meta">15 Feb 2012 @NanJing</p>

今天重装Git。Push的时候需要github帐户的密码，和我以前遇到的情况不太一样。搜了一下，原来是由push的方式决定的，当通过ssh push的时候，就使用ssh key和相应的密码验证，而使用https push就要验证github帐户的密码。只要在 `$REPO_DIR/.git/config` 里把origin的url字段改成 `git@github.com:your_usr_name/your_repo_name.git` 。

然而，即使用ssh key，也会在每次push的时候要求passphrase。为避免繁琐，github提供了一种[方法](http://help.github.com/ssh-key-passphrases/)，通过一个ssh-agent来把你的密码加密保存，在每次需要的时候自动帮你输入。链接中没有提到Windows的设置方法，很简单，把以下内容加到 `$GIT_DIR/etc/profile` 最后即可。
    SSH_ENV="$HOME/.ssh/environment"

    # start the ssh-agent
    function start_agent {
        echo "Initializing new SSH agent..."
        # spawn ssh-agent
        ssh-agent | sed 's/^echo/#echo/' > "$SSH_ENV"
        echo succeeded
        chmod 600 "$SSH_ENV"
        . "$SSH_ENV" > /dev/null
        ssh-add
    }

    # test for identities
    function test_identities {
        # test whether standard identities have been added to the agent already
        ssh-add -l | grep "The agent has no identities" > /dev/null
        if [ $? -eq 0 ]; then
            ssh-add
            # $SSH_AUTH_SOCK broken so we start a new proper agent
            if [ $? -eq 2 ];then
                start_agent
            fi
        fi
    }

    # check for running ssh-agent with proper $SSH_AGENT_PID
    if [ -n "$SSH_AGENT_PID" ]; then
        ps -ef | grep "$SSH_AGENT_PID" | grep ssh-agent > /dev/null
        if [ $? -eq 0 ]; then
        test_identities
        fi
    # if $SSH_AGENT_PID is not properly set, we might be able to load one from
    # $SSH_ENV
    else
        if [ -f "$SSH_ENV" ]; then
        . "$SSH_ENV" > /dev/null
        fi
        ps -ef | grep "$SSH_AGENT_PID" | grep -v grep | grep ssh-agent > /dev/null
        if [ $? -eq 0 ]; then
            test_identities
        else
            start_agent
        fi
    fi
    
其余设置参照链接中。