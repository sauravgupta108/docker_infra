# Python 3 as base image
FROM python:3.6

ARG username=test_user
ARG password=T3stUs3r@1234

ARG ssh_default_port=22
ARG ssh_port=2298

ARG public_key_path=dokr_infra.pub

ARG user_home_dir="/home/${username}"

# Install openssh server in order to enable ssh 
RUN apt update && apt install  openssh-server sudo -y

# Create a super user 
RUN useradd -rm -d ${user_home_dir} -s /bin/bash -g root -G sudo -u 1000 ${username}

# Set password of user
RUN  echo ${username}:${password} | chpasswd

# Change the default ssh port
RUN sed -i "s/#Port ${ssh_default_port}*/Port ${ssh_port}/" /etc/ssh/sshd_config

# Start ssh service
RUN service ssh start

# Enable passwordless ssh to the container

# Copy public key and set appropriate permissions.
RUN mkdir -p "${user_home_dir}/.ssh"

RUN touch "${user_home_dir}/.ssh/authorized_keys"

COPY ${public_key_path} "${user_home_dir}/.ssh/authorized_keys"

RUN chmod 600 "${user_home_dir}/.ssh/authorized_keys"
RUN chmod 700 "${user_home_dir}/.ssh"

RUN chown "${username}:root" -R "${user_home_dir}/.ssh"

CMD ["/usr/sbin/sshd", "-D"]
