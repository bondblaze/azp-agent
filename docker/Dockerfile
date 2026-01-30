FROM ubuntu:24.04
ENV TARGETARCH="linux-x64"
ENV TERRAFORM_VERSION="1.14.4"
ENV HCL_TO_JSON_VERSION="0.6.8"

RUN apt update && \
  apt upgrade -y && \
  apt install -y curl git jq libicu74 wget unzip

# Install terraform
RUN wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip
RUN unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip
RUN mv terraform /usr/bin/terraform

RUN wget https://github.com/tmccombs/hcl2json/releases/download/v${HCL_TO_JSON_VERSION}/hcl2json_linux_amd64.tar.gz
RUN tar -xvzf hcl2json_linux_amd64.tar.gz
RUN mv hcl2json /usr/bin/hcl2json

# Install Azure CLI
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash
RUN az aks install-cli

WORKDIR /azp/

COPY start.sh ./
RUN chmod +x ./start.sh

# Create agent user and set up home directory
RUN useradd -m -d /home/agent agent
RUN chown -R agent:agent /azp /home/agent

USER agent
# Another option is to run the agent as root.
# ENV AGENT_ALLOW_RUNASROOT="true"

ENTRYPOINT [ "./start.sh" ]