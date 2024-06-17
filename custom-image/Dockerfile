FROM quay.io/ansible/awx:24.5.0

# Switch to root user
USER root

# Add the Receptor repository and install Receptor
RUN dnf install -y epel-release && dnf update -y &&  dnf install -y receptor

# Copy the receptor.conf file into the image
COPY receptor.conf /etc/receptor/receptor.conf

# Switch back to the default user
USER awx
