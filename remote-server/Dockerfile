# 1: Ubuntu 24.04 as the base
FROM ubuntu:24.04

# 2: Install everything that Ansible will need and cleanup
RUN apt-get update && apt-get install -y \
        openssh-server \
        python3 \
        sudo && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

# 3: Create the .ssh directory and generate hostkeys
RUN mkdir -p /var/run/sshd && ssh-keygen -A

# 4: Configure SSH for root login
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
        sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config

# 5: Setup key-based auth
RUN mkdir -p /root/.ssh && chmod 700 /root/.ssh
RUN echo "***ADD YOUR OWN SSH KEY HERE***" > /root/.ssh/authorized_keys && \
        chown root:root /root/.ssh/authorized_keys && \
        chmod 600 /root/.ssh/authorized_keys

# 6: Expose the ssh port
EXPOSE 22

# 7: Start the SSH daemon in the foreground with logging
CMD ["/usr/sbin/sshd", "-D", "-e"]