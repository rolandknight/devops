# DevOps Tools Part 5 - Artifactory

## Prerequisites
- **Rancher 1.6**
- **Ports 80, and 443 Exposed** Rancher host with these unused exposed to the internet (access to all other ports via the internet should be blocked).
- **Domain name and name server access** You need a domain name and access to your domain name server to create CNAME and A records.

## Artifactory Installation and Setup

1. **Create DNS Records** You will need to create the following DNS records:

    | Name             | Type  | Data                          |
    | ---------------- | ------| ----------------------------- |
    | *.artifactory    | CNAME |  internal.**\<your domain\>** |
    | artifactory      | A     | **your IPv4**                 |

    The wildcard is for docker registries.

1. **Add DevOps Stack Catalog** Add Rancher Catalog from [here](https://github.com/rolandknight/rancher-catalog.git)

1. **Create Traefik Stack**
    - Traefik ports:
        - 80 -> 80 (http)
        - 443 -> 443 (https)
        - 8000 -> 8000 (Traefik console)
    - Traefik labels:
        - io.rancher.sidekicks=traefik-acme
        - io.rancher.scheduler.global=true
        - io.rancher.container.hostname_override=container_name
        - io.rancher.scheduler.affinity:host_label=traefik_lb=true

1. **Create Artifactory Stack**
    - Create new Artifactory stack and set the following variables:
        - FQDN = artifactory.\<your domain\>
        - Version = PRO
    - Click **Launch**
1. **Basic Configuration**
    - Point your browser to https://artifactory.ionpig.com and login. Initial login is admin/password.
    - Click on **Admin**/**Users** and enter your email address and change the admin password to something a little more secure than password.
    - Click on **Admin**/**Configuration**/**General Configuration** and set the following:
        - Server Name = artifactory.ionpig.com
        - Custom Base URL = https://artifactory.ionpig.com/artifactory
    - Click on **Admin**/**Repositories**/**Local** and click **+New**
    - Select **Docker** repository type and enter:
        - Repository key = docker
    - Note that if you require another docker repository or wish to change the name, you must add/change the Rancher label **traefik.frontend.rule** to contain the hostname including the repo prefix.
1. **Push a Docker Image**
    ```
    docker login https://docker.artifactory.ionpig.com
    docker pull alpine
    docker tag alpine docker.artifactory.ionpig.com/alpine
    ```
