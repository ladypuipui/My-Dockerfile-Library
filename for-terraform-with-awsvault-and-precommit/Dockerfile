ARG TAG=3.10.1-alpine3.15
FROM python:${TAG}

# Install Packages with dependencies
RUN apk add --no-cache \
  git \
  curl \
  pass \
  unzip \
  gpg-agent \
  && python3 -m pip install --no-cache-dir --upgrade pip

# Install aws-vault
RUN curl -sSLo /usr/local/bin/aws-vault https://github.com/99designs/aws-vault/releases/download/v6.3.1/aws-vault-linux-amd64 \
  && chmod 755 /usr/local/bin/aws-vault \
  && echo "export AWS_VAULT_BACKEND=pass" >> ~/.bashrc \
  && echo "export AWS_VAULT_PASS_PREFIX=aws-vault" >> ~/.bashrc \
  && echo "export AWS_SESSION_TOKEN_TTL=3h" >> ~/.bashrc \
  && echo "export GPG_TTY=\$(tty)" >> ~/.bashrc

# Install Terraform
RUN git clone https://github.com/tfutils/tfenv.git ~/.tfenv \
  && echo 'PATH=${HOME}/.tfenv/bin:${PATH}' >> ~/.bashrc
  
# Install tflint
RUN curl -s https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh | bash

# Install terraform-docs
RUN curl -sSLo ./terraform-docs.tar.gz https://github.com/terraform-docs/terraform-docs/releases/download/v0.16.0/terraform-docs-v0.16.0-linux-amd64.tar.gz \
  && tar -C /usr/local/bin/ -xzf terraform-docs.tar.gz \
  && chmod 755 /usr/local/bin/terraform-docs \
  && rm -rf ./terraform-docs.tar.gz

# Install SOPS
RUN curl -sSLo /usr/local/bin/sops https://github.com/mozilla/sops/releases/download/v3.7.1/sops-v3.7.1.linux \
  && chmod 755 /usr/local/bin/sops

# Install AWS CLI
RUN pip3 install awscli --upgrade

# Install pre-commit
RUN pip3 install --no-cache-dir pre-commit

# infracost
RUN  curl -fsSL https://raw.githubusercontent.com/infracost/infracost/master/scripts/install.sh | sh

# Terragrunt
RUN git clone https://github.com/cunymatthieu/tgenv.git ~/.tgenv \
    && echo 'export PATH="$HOME/.tgenv/bin:$PATH"' >> ~/.bashrc \
    && source ~/.bashrc

# Terrascan
RUN curl -L  https://github.com/tenable/terrascan/releases/download/v1.15.2/terrascan_1.15.2_Linux_x86_64.tar.gz > terrascan.tar.gz \
    && tar -xf terrascan.tar.gz terrascan && rm terrascan.tar.gz \
    && install terrascan /usr/local/bin && rm terrascan

# # TFSec
RUN curl -sSLo /usr/local/bin/tfsec https://github.com/aquasecurity/tfsec/releases/download/v1.26.2/tfsec-linux-amd64 \
  && chmod +x /usr/local/bin/tfsec

WORKDIR /terraform