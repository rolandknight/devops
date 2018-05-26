# DevOps Tools Part 3 - Rancher

## Install Rancher

1. Check that your docker version is supported [here](https://rancher.com/docs/rancher/v1.6/en/hosts/).

1. Install the Rancher server:
    ```
    sudo docker run -d --restart=unless-stopped -p 8080:8080 rancher/server:v1.6.17
    ```
1. Open Rancher server console: Open a browser at **http://\<your server\>:8080** and enjoy!

## Setup Access Control

1. Select **ADMIN** -> **Access Control**
1. Click **Local**
1. Enter **Username** and **Password**
1. Click **Enable Local Auth**

## Add Rancher Host

1. Select **ADMIN** -> **Settings**
1. In the **Host Registration URL** page select **Something else** and enter URL for this machine (localhost won't cut it!). Be sure to include the port. NAT address is fine if reachable from the Rancher host machines. And here's a quick way to get your internal IP address if you're not certain:
   ```
   ip route get 1.1.1.1 | awk '{print $NF; exit}'
   ```
1. Select **INFRASTRUCTURE** -> **Hosts**
1. Click **Add Host**
1. Select **Custom** (should already be selected)
1. In item **4** and enter the **Rancher host IP** address reachable by the host (in this case the same IP).
1. In item **5** click the **copy icon** to copy the registration command to the clipboard. It will look something like this:
   ```
   sudo docker run -e CATTLE_AGENT_IP="192.168.0.100"  --rm --privileged -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/rancher:/var/lib/rancher rancher/agent:v1.2.10 http://192.168.0.100:8085/v1/scripts/00000000000000000000:0000000000000:00000000000000000000000000
   ```
1. Open a shell on your host, paste the command above, and hit enter. Then wait a few seconds and your host should be registered with the Rancher server.
   If successful, the last line displayed in the shell should look like this:
   ```
   INFO: Launched Rancher Agent: 46a328afd7c4a755b9d79c397724e8940b1344ea5be761989bb18d8eeed6c3b3
   ```
1. Now your **INFRASTRUCTURE** -> **Hosts** page will show your shiny new host ready deployment of docker containers!

## Deploy a Stack

1. Select **CATALOG** -> **Community** to browse the Ranche community catalog in Github
1. Let's use the fabulous Prometheus stack as an example to deploy. Find the **Prometheus** app and click **View Details**
1. Scroll down and click **Launch**. Note that here you can specify app deployment parameters including ports. For now defaults are fine.
1. Wait a few seconds until the apps are running, then point your browser to
   **http://\<your host IP\>:3000** to see the Prometheus/Grafana console.
   Select **Host Stats** to see the pretty graphical host statistics.  
1. The Prometheus stack is great and useful but, since it's so easy to re-deploy, let's remove it! Simply select **STACKS** -> **User**, find the **Prometheus** stack, click the **three dots**
   to the far right and select **Delete**.

## Whats Next
