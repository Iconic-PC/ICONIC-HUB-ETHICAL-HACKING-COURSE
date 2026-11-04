# OWASP Juice Shop --- Download, Verify, and Run on Kali Linux

**Author:** Iconic PC\
**Platform:** Kali Linux\
**Deployment method:** Docker\
**Application:** OWASP Juice Shop\
**Purpose:** Set up a local web application security training lab

> **Scope and safety:** This guide runs Juice Shop locally and binds it
> to `127.0.0.1`, so it is not intentionally exposed to your network or
> the internet. Use your own local instance for scanning and
> exploitation practice. Do not scan the public Juice Shop demo.

------------------------------------------------------------------------

## 1. Open Kali Linux Terminal

Press:

``` text
Ctrl + Alt + T
```

This opens the terminal.

## 2. Check whether Docker is installed

Run:

``` bash
docker --version
```

Example output:

``` text
Docker version 27.x.x
```

The exact version number may be different.

-   If a Docker version appears, Docker is installed.
-   If you see `command not found`, Docker is not installed or is not
    available in your PATH.

## 3. Check whether the Docker service is running

Run:

``` bash
sudo systemctl status docker
```

Look for:

``` text
Active: active (running)
```

If Docker is installed but inactive, start it:

``` bash
sudo systemctl start docker
```

Then check its status again:

``` bash
sudo systemctl status docker
```

If your system does not use systemd, the service command may differ.

## 4. Check whether the Juice Shop image is already downloaded

Run:

``` bash
docker images
```

Look for:

``` text
bkimminich/juice-shop
```

Example:

``` text
REPOSITORY                 TAG       IMAGE ID       SIZE
bkimminich/juice-shop      latest    abc123...      ...
```

Interpretation:

-   If `bkimminich/juice-shop` appears, the image is already downloaded
    locally.
-   If it does not appear, continue to Step 5.

If Docker reports a permission error, try the command with `sudo`:

``` bash
sudo docker images
```

## 5. Download the OWASP Juice Shop image

If the image is not present, run:

``` bash
docker pull bkimminich/juice-shop
```

If you need elevated permissions, use:

``` bash
sudo docker pull bkimminich/juice-shop
```

Wait for Docker to finish downloading the image. A successful pull
usually ends with a message indicating that the image is up to date or
downloaded.

## 6. Verify the download

Run:

``` bash
docker images
```

You should now see:

``` text
bkimminich/juice-shop
```

This confirms that the Docker image is stored locally.

**Important:** A downloaded image is not the same as a running
application. You still need to create and start a container.

## 7. Check whether a Juice Shop container already exists

Run:

``` bash
docker ps -a
```

Look for a container named:

``` text
juice-shop
```

-   `docker ps` shows running containers.
-   `docker ps -a` shows running and stopped containers.

If a Juice Shop container is already running, you may not need to create
another one.

## 8. Start Juice Shop locally

If the image is downloaded and no existing container needs to be reused,
run:

``` bash
docker run --rm -d \
  --name juice-shop \
  -p 127.0.0.1:3000:3000 \
  bkimminich/juice-shop
```

What the options mean:

  -----------------------------------------------------------------------
  Option                              Meaning
  ----------------------------------- -----------------------------------
  `--rm`                              Remove the container after it stops

  `-d`                                Run the container in the background

  `--name juice-shop`                 Give the container the name
                                      `juice-shop`

  `-p 127.0.0.1:3000:3000`            Publish container port 3000 only on
                                      this machine's loopback interface

  `bkimminich/juice-shop`             The image used to create the
                                      container
  -----------------------------------------------------------------------

If Docker permission is denied, repeat the command with `sudo` before
`docker`.

## 9. Confirm that the container is running

Run:

``` bash
docker ps
```

Look for a container named `juice-shop` and a port mapping similar to:

``` text
127.0.0.1:3000->3000/tcp
```

If it does not appear, inspect all containers:

``` bash
docker ps -a
```

Then inspect the application logs:

``` bash
docker logs juice-shop
```

## 10. Open Juice Shop in your browser

Open Firefox in Kali Linux and visit:

``` text
http://127.0.0.1:3000
```

You can also test the HTTP response from the terminal:

``` bash
curl -I http://127.0.0.1:3000
```

A successful response commonly includes a status such as `200 OK`. The
exact response can vary.

## 11. Configure target variables for later exercises

Once Juice Shop is running, define these variables in the same terminal
session:

``` bash
export TARGET="127.0.0.1"
export PORT="3000"
export URL="http://${TARGET}:${PORT}"
```

Verify them:

``` bash
echo "$TARGET"
echo "$PORT"
echo "$URL"
```

Expected output:

``` text
127.0.0.1
3000
http://127.0.0.1:3000
```

These commands only define shell variables. They do not send requests to
the application.

Check the application:

``` bash
curl -I "$URL"
```

The variables are temporary and need to be defined again in a new
terminal session.

------------------------------------------------------------------------

## 12. Troubleshooting

### `docker: command not found`

Docker may not be installed or may not be in your PATH. Install Docker
using the official instructions for your Kali/Debian environment, then
reopen the terminal and check:

``` bash
docker --version
```

### Cannot connect to the Docker daemon

Check the service:

``` bash
sudo systemctl status docker
```

Start it if necessary:

``` bash
sudo systemctl start docker
```

### Permission denied when running Docker

Try the command with `sudo`, for example:

``` bash
sudo docker images
```

Do not make broad permission changes on a shared machine just to avoid
using `sudo`.

### Container name is already in use

Check existing containers:

``` bash
docker ps -a
```

If the existing `juice-shop` container is running, use it. If it is
stopped and you no longer need it, remove it:

``` bash
docker rm juice-shop
```

Then start the container again.

### Port 3000 is already in use

Check what is listening:

``` bash
sudo ss -lntp | grep ':3000'
```

If another application is using that port, either stop that application
if appropriate or choose a different host port. For example, to use host
port 3001:

``` bash
docker run --rm -d \
  --name juice-shop \
  -p 127.0.0.1:3001:3000 \
  bkimminich/juice-shop
```

Then use:

``` text
http://127.0.0.1:3001
```

Update the target variables accordingly:

``` bash
export TARGET="127.0.0.1"
export PORT="3001"
export URL="http://${TARGET}:${PORT}"
```

### Browser says the site cannot be reached

Check the container:

``` bash
docker ps
```

Check its logs:

``` bash
docker logs juice-shop
```

Check the port:

``` bash
sudo ss -lntp | grep ':3000'
```

### `curl` reports connection refused

Possible causes include:

-   The container is stopped.
-   The application is still starting.
-   The port mapping is incorrect.
-   The application failed during startup.

Check `docker logs juice-shop` before changing settings.

------------------------------------------------------------------------

## 13. Stop the lab

To stop the running container:

``` bash
docker stop juice-shop
```

Because the container was started with `--rm`, Docker removes the
container after it stops. The downloaded image remains available
locally.

To confirm that the image remains:

``` bash
docker images
```

------------------------------------------------------------------------

## 14. Quick command reference

  Command                               Purpose
  ------------------------------------- -------------------------------
  `docker --version`                    Check Docker installation
  `sudo systemctl status docker`        Check Docker service
  `sudo systemctl start docker`         Start Docker service
  `docker images`                       List downloaded images
  `docker pull bkimminich/juice-shop`   Download Juice Shop image
  `docker ps`                           List running containers
  `docker ps -a`                        List all containers
  `docker logs juice-shop`              View Juice Shop logs
  `docker stop juice-shop`              Stop the lab
  `curl -I http://127.0.0.1:3000`       Check the local HTTP response

## 15. Next steps after installation

After confirming that Juice Shop opens in your browser, you can begin
the practical assessment:

1.  Reconnaissance and application mapping.
2.  Scanning and enumeration.
3.  Vulnerability assessment with controlled tools.
4.  Manual web testing with Burp Suite.
5.  SQL injection and other intentionally vulnerable challenge
    exercises.
6.  Validation, evidence collection, and reporting.
7.  Remediation and retesting.

Start with the local instance. Keep the host binding on `127.0.0.1`
unless you deliberately build an isolated lab network and understand the
exposure.

------------------------------------------------------------------------

## Learning resources

-   OWASP Juice Shop: https://owasp.org/www-project-juice-shop/
-   Juice Shop companion guide: https://pwning.owasp-juice.shop/
-   Juice Shop GitHub repository:
    https://github.com/juice-shop/juice-shop
-   Docker documentation: https://docs.docker.com/

------------------------------------------------------------------------

**Done by: Iconic PC**

*Build the lab. Verify the target. Test responsibly. Document what you
learn.*
