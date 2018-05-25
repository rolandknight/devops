# DevOps Tools Part 4 - GitLab

## GitLab Installation Step By Step

## Prerequisites

- **Rancher 1.6**
- **Ports 80, 222, 443, and 5005 Exposed** Rancher host with these unused exposed to the internet (access to all other ports via the internet should be blocked).
- **Domain name and name server access** You need a domain name and access to your domain name server to create CNAME and A records.

## Basic GitLab Setup

1. **Create DNS Records** You will need to create the following DNS records:

    | Name             | Type  | Data  |
    | ---------------- | ------| ----- |
    | @                | A     | **your IPv4** |
    | *                | CNAME | **\<your domain\>** |
    | *.internal       | CNAME |  internal.**\<your domain\>** |
    | internal         | A     | **your IPv4**     |
    | rancher.internal | A     | **your IPv4** |

    Note that this works for subdomains as well. Simply replace the @ with your subdomain (ie devops.mydomain.com).

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
1. **Create GitLab Stack**
    - Create new GitLab stack (be sure to set your FQDN to gitlab.\<your domain\>!)
    - SSH into the container and modify */etc/gitlab/gitlab.rb* as follows:
        - external_url 'https://gitlab.\<your domain\>'
        - gitlab_rails['gitlab_shell_ssh_port'] = 222
        - registry_external_url 'https://gitlab.\<your domain\>:5005'
        - registry['log_directory'] = "/var/log/gitlab/registry"
        - registry['log_level'] = "debug"
        - nginx['listen_port'] = 80
        - nginx['listen_https'] = false
    - Container labels:
        - traefik.port=80
        - traefik.frontend.rule=Host:gitlab.\<your domain\>
        - traefik.enable=true
        - io.rancher.container.hostname_override=container_name
    - GitLab ports:
        - SSH: 222 -> 22 (SSH)
        - 5005 -> 5005 (Docker registry)
        - 8008 -> 80 (Direct http access)
1. **Savour GitLab!**
    Enjoy the beauty of GitLab! On first login you will need to set the admin username and password. Be aware that this instance IS exposed to the internet - so use good passwords! Once the SSL cert has been downloaded from [Let's Encrypt](https://letsencrypt.org/), you can close the ports from internet access but you will need to manually renew your HTTPS certificate every 30 days (not recommended).
