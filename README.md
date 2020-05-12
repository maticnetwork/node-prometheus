# Matic node Monitoring

Monitor your Heimdall, Bor and Virtual machine node with Prometheus and Grafana

## Follow below steps to set up monitoring for you Matic Node

**Pre-Requesite**: You need to make sure that you Matic Node is setup, including Heimdall and Bor nodes. 

**Step 1:**

You will need to change `prometheus` flag to `true` in the `config.toml`. 

To access the config.toml here:

**Linux Packages**: `sudo vi /etc/heimdall/config/config.toml`
**Binaries**: `vi ~/.heimdalld/config/config.toml`

By default the prometheus flag is set to `false`.

Make sure that you keep the formatting intact.

**Step 2:**

Once you have made the changes in the `config.toml` you will need to stop Heimdall service and restart it.

To stop heimdall service:

**Linux Packages**: `sudo service heimdalld stop`
**Binaries**: `pkill heimdalld`

To restart heimdall service:

**Linux Packages**: `sudo service heimdalld start`
**Binaries**: `heimdalld start`

**Step 3:**

You will make changes in the Bor service file. To access the `bor.service` file you can run this command:

`locate bor.service`

There would be multiple entries. You will need to edit the file in this system path: `/etc/systemd/system/bor.service`

To edit the service file you can run the following command:

`sudo nano /etc/systemd/system/bor.service`

Now in this you would see `ExecStart=/bin/bash` with multiple paramaters in line to it. Add this, `--metrics --pprof --pprofport 7071 --pprofaddr 0.0.0.0` to this line of paramaters. You can add it anywhere, for example, you can add it after `--maxpeers 150`

You need to make sure that the spaces and formatting are intact.

Now you will notice that your bor will stop because we made changes to the service file. You will need to stop the bor service and restart it.

To Stop Bor service:

**Linux Packages**: `sudo service bor stop`
**Binaries**: `pkill bor`

Note: When you stop the service of Bor you may encounter a warning asking you run `systemctl daemon-reload` to reload units.

You will need to run this as `sudo systemctl daemon-reload`. Once this is successfull you can proceed with starting the Bor service.

To Restart Bor service:

**Linux Packages**: `sudo service bor start`
**Binaries**: `bash start.sh <Your Ethereum wallet address>`

Note: You will need to use the same Address that you used when you had initially set up your node.

**Step 4:**

Clone the Prometheus repository to your local machine

`git clone https://github.com/maticnetwork/node-prometheus.git`

And then switch to the Prometheus directory.

Then install Docker by running the following command:

`sudo apt install docker-compose`

Once Docker is installed then you run docker by running the following command to start Prometheus: `docker-compose up -d`

**Step 5:**

Open Grafana at following URL:

```
http://host_ip:3000
```

**Creating a Proxy Tunnel**

**This is an optional step.**

If you're running your Node on a Cloud service then you can also create a proxy tunnel on your local machine to your Cloud Machine

You can run this command `ssh -N -L 3000:0.0.0.0:3000 ubuntu@<your ip address>`. There would not be any output for this, however you can then check in your browser and access this URL: <your ip address>:3000

You should be able to login to Grafana dashboard. Else you can always use http://localhost:3000

## Grafana default login details

These login credentials can be changed according to user preferences once logged in:

```
username: admin
password: admin
```

## Grafana datasource configuration and navigation snapshots

Grafana uses web based APIs to connect to prometheus server for indexed data. For that, it needs prometheus host name.


1. Open grafana at url: http://host_ip:3000. Hover on the setting icon in the left pane and selet Data Sources:



    ![Screenshot 2020-04-03 at 4 49 47 PM](https://user-images.githubusercontent.com/31979627/78356085-8bf3a480-75cc-11ea-9ed0-635edd495c96.png)


2. Notice that Prometheus sample datasource is added and click on the same:


     ![Screenshot 2020-04-03 at 4 50 14 PM](https://user-images.githubusercontent.com/31979627/78356289-e856c400-75cc-11ea-86da-e94d742a07f7.png)


3. Change the HTTP url to http://host_ip:9090 and save. After the success message, go to Grafana home:


     ![Screenshot 2020-04-03 at 5 14 53 PM](https://user-images.githubusercontent.com/31979627/78357564-4dabb480-75cf-11ea-9c9c-f6e8daadec47.png)


4. Click on the Home button on the left top and select the required option (Heimdall-Dashboard/Bor-Dashboard/Node-Dashboard) to view the respective metrics.


     ![Screenshot 2020-04-03 at 5 39 36 PM](https://user-images.githubusercontent.com/31979627/78359766-543c2b00-75d3-11ea-8b62-d8e8ee422191.png)

5. Notice Heimdall-Dashboard loaded as below

     ![Screenshot 2020-04-03 at 5 46 49 PM](https://user-images.githubusercontent.com/31979627/78359855-78980780-75d3-11ea-8cdf-8db0cb5ac4cc.png)

6. Notice Bor-Dashboard loaded as below
     
     ![Screenshot 2020-04-07 at 1 23 47 PM](https://user-images.githubusercontent.com/31979627/78644246-33c1e880-78d3-11ea-9073-afe8077ab917.png)
     
7. Notice Node-Dashboard loaded as below

     ![Screenshot 2020-04-07 at 1 23 18 PM](https://user-images.githubusercontent.com/31979627/78644461-89969080-78d3-11ea-9123-8587653c9d9a.png)
     

