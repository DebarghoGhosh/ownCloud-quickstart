# Overview

ownCloud is an open-source software which allows you to sync, share and collaborate files and content with your team. This helps a team to seamlessly work on data from anywhere and on any device. Some of the major features of ownCloud are:

*   Flexible and secure sharing of files and folders.
*   Collaborate remotely in real time irrespective of the platform.
*   Access, edit and share documents from mobile devices.
*   Share files directly from the file manager using the Virtual file system to save storage and bandwidth.
*   Prevent conflicts while editing by locking the files to ensure better productivity.

In this guide, we will learn how to install, configure, administer and connect to the ownCloud server.


# Installation  

In this section we will learn how to install ownCloud using a Docker Image running on a single Ubuntu 18.04 LTS machine. This helps you to install and set up the ownCloud server quickly and easily. If you prefer the manual installation process please refer to the detailed [Admin Manual](https://doc.owncloud.org/server/10.5/admin_manual/installation/).


## Prerequisites

Note: If you are already using Ubuntu 18.04 and have Docker installed you can skip these steps.  

1. Verify the version of Ubuntu running the following command:

    ```
    $ lsb_release -a
    ```
    The output will look like:

```
No LSB modules are available.
    Distributor ID: Ubuntu
Description:    Ubuntu 18.04.3 LTS
Release:        18.04

```

2. Verify if Docker is installed by running the following command:

    ```
    $ docker
    ```
    The output will look like:

    ```  
    Usage:  docker [OPTIONS] COMMAND

    A self-sufficient runtime for containers

    Options:
      --config string      Location of client config files (default "/home/datadebargho/.docker")
      -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context
                           set with "docker context use")
      -D, --debug              Enable debug mode
      -H, --host list          Daemon socket(s) to connect to
      -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
          --tls                Use TLS; implied by --tlsverify
          --tlscacert string   Trust certs signed only by this CA (default "/home/datadebargho/.docker/ca.pem")
          --tlscert string     Path to TLS certificate file (default "/home/datadebargho/.docker/cert.pem")
         --tlskey string      Path to TLS key file (default "/home/datadebargho/.docker/key.pem")
         --tlsverify          Use TLS and verify the remote
      -v, --version            Print version information and quit

    Management Commands:
    builder     Manage builds
    config      Manage Docker configs
    container   Manage containers
    context     Manage contexts
    engine      Manage the docker engine
	...
    ```

# Install and Configure

 If docker is not installed you have to install docker. Follow the steps given below:

1. Create a project folder and navigate to the folder by executing the following commands:

    ```
    $ mkdir docker_install
    $ cd docker_install
    ```


2. Next, download the `docker-compose.yml` file from the ownCloud GitHub repository by running the following command:

    ```
    $ wget https://raw.githubusercontent.com/owncloud/docs/master/modules/admin_manual/examples/installation/docker/docker-compose.yml

    --2020-09-01 01:08:01--  https://raw.githubusercontent.com/owncloud/docs/master/modules/admin_manual/examples/installation/docker/docker-compose.yml
    Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.152.133
    Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.152.133|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 1650 (1.6K) [text/plain]
    Saving to: 'docker-compose.yml'

    docker-compose.yml                 100%[=============================================================>]   1.61K  --.-KB/s    in 0s

    2020-09-01 01:08:01 (24.2 MB/s) - 'docker-compose.yml' saved [1650/1650]
    ```


3. Create a .env file by executing the following command:

	

	`$ nano .env`

    Copy the following environment configuration:

```
    OWNCLOUD_VERSION=latest
    OWNCLOUD_DOMAIN=localhost
    ADMIN_USERNAME=admin
    ADMIN_PASSWORD=admin
```

Save the configuration.

Note: If you change the values of the environment parameters for domain, port, username, and password, in the subsequent steps, you must use the same values for launching the web UI and logging in to the ownCloud server.

4. Use the `docker-compose` CLI tool to build and start the container by running the following command:

```
$ docker-compose up -d
```

The output will look like:

```
    Creating network "owncloudserverdocker_default" with the default driver
    Creating volume "owncloudserverdocker_files" with local driver
    Creating volume "owncloudserverdocker_redis" with local driver
    Creating volume "owncloudserverdocker_backup" with local driver
    Creating volume "owncloudserverdocker_mysql" with local driver
    Pulling redis (webhippie/redis:latest)...
    latest: Pulling from webhippie/redis
    ae4a0e1e8235: Pull complete
    1e24c34b24d1: Pull complete
    95f9aef2e99d: Pull complete
    9641411ccde6: Pull complete
    3c5b5eddcd67: Pull complete
    Digest: sha256:42f6d51be6a7a5ef6fb672e98507824816566f0b1f89c19b2d585f54e26b2529
    Status: Downloaded newer image for webhippie/redis:latest
    Pulling db (webhippie/mariadb:latest)...
    latest: Pulling from webhippie/mariadb
    ae4a0e1e8235: Already exists
    1e24c34b24d1: Already exists
    95f9aef2e99d: Already exists
    bd3d546db57d: Pull complete
    ce65dc9183aa: Pull complete
    Digest: sha256:8a2c927529e5fd6238f08f79e3855d90a353e4475481574aa4bf0b90550b5db9
    Status: Downloaded newer image for webhippie/mariadb:latest
    Creating owncloudserverdocker_db_1 ...
    Creating owncloudserverdocker_redis_1 ...
    Creating owncloudserverdocker_db_1
    Creating owncloudserverdocker_redis_1 ... done
    Creating owncloudserverdocker_owncloud_1 ...
```


Note: If you do not have docker-compose installed, install it by running the following command:

```
$ sudo apt install docker-compose

```

5. Now verify the status of the processes to determine the server is up and running. Execute the following command:

	`$ docker-compose ps`

	The output will look like:


```
         Name                            Command               State           Ports
-------------------------------------------------------------------------------------------------
owncloudserverdocker_db_1         /usr/bin/entrypoint /bin/s ...   Up      3306/tcp
owncloudserverdocker_owncloud_1   /usr/bin/entrypoint /usr/b ...   Up      0.0.0.0:8080->8080/tcp
owncloudserverdocker_redis_1      /usr/bin/entrypoint /bin/s ...   Up      6379/tcp
```

Now that the installation is done, you can test the docker image to verify and open ownCloud on the web.

# Test Docker Image

To quickly test the docker image follow the steps below:


1. Download the official docker image for ownCloud server latest version by running the following command:

    ```
    $ docker pull owncloud/server:latest
    ```

2. Once the image is downloaded run the image on port 8080 by executing the following command:

    ```
    $ docker run -p8080:8080 owncloud/server:latest

    ```

Once the apache demon starts enter [http://localhost:8080](http://localhost:8080) on your preferred browser. The following screen will appear:

<p id="gdcalert12" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert13">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image1.png "image_tooltip")

# Administration

In this section, we will learn about administration workflows like:

*   Allow access to the server using IP address and port number
*   Add a user account

## Allow Access to Server

Once the ownCloud server is up and running with default environment values, you can access it using your browser. Follow the steps below:

1. Enter [http://localhost:8080](http://localhost:8080) on your preferred browser. The login page appears as shown below:

<p id="gdcalert13" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert14">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image2.png "image_tooltip")

2. Enter the username and password as configured in the .env file. The default is admin/admin: 

<p id="gdcalert14" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert15">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")


3. The Home screen appears, skip the message for installing desktop and mobile clients. The files page is displayed as shown below:   

<p id="gdcalert15" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert16">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image4.png "image_tooltip")

## Add user Account

To create a user account follow the steps given below:

1. Login to the ownCloud server using admin credentials. Click the Admin drop-down at the top-right corner of the screen and select Users. Refer to the image below: 

<p id="gdcalert16" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image5.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert17">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image5.png "image_tooltip")

2. The screen with the list of users appears as shown below: 

<p id="gdcalert17" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image6.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert18">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image6.png "image_tooltip")


3. Enter the Username and E-mail in the respective text boxes and click Create as shown below:   

<p id="gdcalert18" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image7.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert19">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image7.png "image_tooltip")


4. Once the user is created it will appear in the list as shown below:    

<p id="gdcalert19" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image8.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert20">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image8.png "image_tooltip")



    After the user is created the user will receive an email with the username and a system generated password.

5. You can assign the user to a group from the Groups drop-down as shown below:

    

<p id="gdcalert20" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image9.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert21">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image9.png "image_tooltip")

# Connect to ownCloud server

As an end user, you can access the ownCloud server from any modern web browsers and compatible device. In order to connect and use ownCloud efficiently, it is  recommended to download and install the desktop and/or the mobile applications (clients). In this section we will learn the easiest way to _connect to the ownCloud server from different clients_.

## Web UI

To access ownCloud server from the web and follow the steps below:

1. Open a tab from any browser.
2. Enter the server URL, or the server's IP address and port number.
3. Enter the login credentials.
4. Start using the web interface. Refer to the image below:

<p id="gdcalert21" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image10.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert22">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image10.png "image_tooltip")

You can now start using the web UI by uploading documents and sharing files.

## Desktop

To start using the ownCloud desktop client for Windows, Mac OS and Linux download the respective desktop client from [here](https://owncloud.com/desktop-app/).  For Linux users installation steps depend on the specific distribution. For more details click [here](https://software.opensuse.org/download/package?project=isv:ownCloud:desktop&package=owncloud-client).

In this guide we will see how to install the desktop client using the installation wizard. Follow the steps given below:

1. After downloading run the wizard. Enter the URL the server of your ownCloud and click Next as shown below:

<p id="gdcalert22" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image11.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert23">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image11.png "image_tooltip")


2. Enter the login credentials, and click Next as shown below:

<p id="gdcalert23" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image12.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert24">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image12.png "image_tooltip")

3. Select the respective folders and files in your local machine to sync with server and click Connect as shown below:
  
<p id="gdcalert24" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image13.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert25">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image13.png "image_tooltip")

For more details on installation, configuration, and usage of the desktop client, click [here](https://doc.owncloud.com/desktop/). 

You can also set up ownCloud on mobile devices. If you are an Android user refer to [ownCloud - Android](https://doc.owncloud.com/android/) document and for iOS refer to the [ownCloud - iOS](https://doc.owncloud.com/ios/).
