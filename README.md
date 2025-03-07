# Velociraptor EDR Deployment Guide

This guide provides step-by-step instructions for deploying the Velociraptor EDR solution. It covers the process of generating configuration files, deploying the server, and installing the client on both Linux and Windows machines.

## Prerequisites

- A Linux server for the Velociraptor server
- Client machines running either Linux or Windows
- Administrative privileges on all machines
- Velociraptor binaries and configuration files

## 1. Generating Configs

To generate the necessary configuration file for the Velociraptor server, run the following command:

```bash
sudo ./velociraptor-x.x.x-linux-amd64 config generate -i
```

This will generate the required configuration file (`server.config.yaml`) for the server and based on the server.config.yaml, a (`client.config.yaml`).

Now we need to make a small change in `server.config.yaml` and expose to GUI to `zero-address`.

```bash
nano server.config.yaml
```

and change all
```bash
  bind_address: 127.0.0.1
```
to
```bash
  bind_address: 0.0.0.0
```

## 2. Deploying the Server

To deploy the Velociraptor server on a Debian-based system, follow these steps:

1. Run the following command to generate the `.deb` package:

```bash
sudo ./velociraptor-linux-amd64 --config server.config.yaml debian server
```

This will create a `.deb` package of Velociraptor for installation.

2. Install the Velociraptor server using the generated `.deb` package:

```bash
sudo apt install velociraptor_server_amd64.deb
```
or
```bash
sudo dpkg -i velociraptor_server_amd64.deb
```

3. After installation, ship the `client` binary file to the client machines.

## 3. Installing the Client

On Windows machines, you need to install two components:

- The Velociraptor binary for the agent, available on the GitHub repository
- The `client.config.yaml` configuration file

If you do not have the `client.config.yaml` file, you can retrieve it from the server using SCP:

```bash
scp username@192.168.x.x:~/edr/client.config.yaml .
```

### 3.1 Attaching the Windows Client to the EDR Server

1. On the Windows machine, run the client installer as an Administrator:

```bash
.\velociraptor-windows-amd64.exe --config client.config.yaml service install
```

### OR

1. Create a repacked executable of velociraptor-client.exe

```bash
./velociraptor-linux-amd64 config repack --exe velocipator-windows-amd64.exe client.config.yaml client_velociraptor.exe
```

2. Now lets move the repacked velociraptor.exe to client either by http.server module of python or any other manual method.

```bash
python3 -m http.server 6969
```

3. Install the client velociraptor in the client machine.

```bash
.\client_velociraptor.exe service install
```

This will install the Velociraptor client as a Windows service, enabling it to connect to the EDR server.

### 3.2 Attaching the Linux Client to the EDR Server

1. On the Windows machine, run the client installer as an Administrator:

```bash
.\velociraptor-windows-amd64.exe --config client.config.yaml service install
```

### OR

1. Create a repacked executable of velociraptor-client.exe

```bash
./velociraptor-linux-amd64 config repack --exe velocipator-windows-amd64.exe client.config.yaml client_velociraptor.exe
```

2. Now lets move the repacked velociraptor.exe to client either by http.server module of python or any other manual method.

```bash
python3 -m http.server 6969
```

3. Install the client velociraptor in the client machine.

```bash
.\client_velociraptor.exe service install
```

This will install the Velociraptor client as a Windows service, enabling it to connect to the EDR server.

## 4. Accessing the Dashboard

Once the server and clients are set up, you can access the Velociraptor dashboard via a web browser. The dashboard will be accessible at the following URL:

```
https://192.168.x.x:8889/
```

From here, you can monitor and manage the connected clients.

## 5. Generating API config file:

Once the server and client are properly deployed and setup, the api.config file needs to be generated for api based usage in Shuffle etc.

```bash
./velociraptor-linux-amd64 --config server.config.yaml config api_client --name soar@cydea.tech --role administrator,api api.config.yaml
```

## 6. Additional Resources

- [Velociraptor GitHub Repository](https://github.com/Velocidex/velociraptor)
- [Velociraptor Documentation](https://docs.velociraptor.app/docs/)
- [Getting started with Velociraptor](https://medium.com/@0D0AResearch/getting-started-with-velociraptor-2f20de22b491)
- [Getting started with Velociraptor: Part 2](https://medium.com/@0D0AResearch/getting-started-with-velociraptor-part-2-b0d1aabe74e1)


## Conclusion

You have now successfully deployed Velociraptor EDR on your network. The dashboard allows you to manage and monitor all connected clients, and the server will ensure that your clients are securely connected and functioning as expected.
# Velociraptor EDR Deployment Guide

This guide provides step-by-step instructions for deploying the Velociraptor EDR solution. It covers the process of generating configuration files, deploying the server, and installing the client on both Linux and Windows machines.

## Prerequisites

- A Linux server for the Velociraptor server
- Client machines running either Linux or Windows
- Administrative privileges on all machines
- Velociraptor binaries and configuration files

## 1. Generating Configs

To generate the necessary configuration file for the Velociraptor server, run the following command:

```bash
sudo ./velociraptor-x.x.x-linux-amd64 config generate -i
```

This will generate the required configuration file (`server.config.yaml`) for the server and based on the server.config.yaml, a (`client.config.yaml`).

Now we need to make a small change in `server.config.yaml` and expose to GUI to `zero-address`.

```bash
nano server.config.yaml
```

and change all
```bash
  bind_address: 127.0.0.1
```
to
```bash
  bind_address: 0.0.0.0
```

## 2. Deploying the Server

To deploy the Velociraptor server on a Debian-based system, follow these steps:

1. Run the following command to generate the `.deb` package:

```bash
sudo ./velociraptor-linux-amd64 --config server.config.yaml debian server
```

This will create a `.deb` package of Velociraptor for installation.

2. Install the Velociraptor server using the generated `.deb` package:

```bash
sudo apt install velociraptor_server_amd64.deb
```
or
```bash
sudo dpkg -i velociraptor_server_amd64.deb
```

3. After installation, ship the `client` binary file to the client machines.

## 3. Installing the Client

On Windows machines, you need to install two components:

- The Velociraptor binary for the agent, available on the GitHub repository
- The `client.config.yaml` configuration file

If you do not have the `client.config.yaml` file, you can retrieve it from the server using SCP:

```bash
scp username@192.168.x.x:~/edr/client.config.yaml .
```

### 3.1 Attaching the Windows Client to the EDR Server

1. On the Windows machine, run the client installer as an Administrator:

```bash
.\velociraptor-windows-amd64.exe --config client.config.yaml service install
```

### OR

1. Create a repacked executable of velociraptor-client.exe

```bash
./velociraptor-linux-amd64 config repack --exe velocipator-windows-amd64.exe client.config.yaml client_velociraptor.exe
```

2. Now lets move the repacked velociraptor.exe to client either by http.server module of python or any other manual method.

```bash
python3 -m http.server 6969
```

3. Install the client velociraptor in the client machine.

```bash
.\client_velociraptor.exe service install
```

This will install the Velociraptor client as a Windows service, enabling it to connect to the EDR server.

### 3.2 Attaching the Linux Client to the EDR Server

1. On the Windows machine, run the client installer as an Administrator:

```bash
.\velociraptor-windows-amd64.exe --config client.config.yaml service install
```

### OR

1. Create a repacked executable of velociraptor-client.exe

```bash
./velociraptor-linux-amd64 config repack --exe velocipator-windows-amd64.exe client.config.yaml client_velociraptor.exe
```

2. Now lets move the repacked velociraptor.exe to client either by http.server module of python or any other manual method.

```bash
python3 -m http.server 6969
```

3. Install the client velociraptor in the client machine.

```bash
.\client_velociraptor.exe service install
```

This will install the Velociraptor client as a Windows service, enabling it to connect to the EDR server.

## 4. Accessing the Dashboard

Once the server and clients are set up, you can access the Velociraptor dashboard via a web browser. The dashboard will be accessible at the following URL:

```
https://192.168.x.x:8889/
```

From here, you can monitor and manage the connected clients.

## 5. Generating API config file:

Once the server and client are properly deployed and setup, the api.config file needs to be generated for api based usage in Shuffle etc.

```bash
./velociraptor-linux-amd64 --config server.config.yaml config api_client --name soar@cydea.tech --role administrator,api api.config.yaml
```

## 6. Additional Resources

- [Velociraptor GitHub Repository](https://github.com/Velocidex/velociraptor)
- [Velociraptor Documentation](https://docs.velociraptor.app/docs/)
- [Getting started with Velociraptor](https://medium.com/@0D0AResearch/getting-started-with-velociraptor-2f20de22b491)
- [Getting started with Velociraptor: Part 2](https://medium.com/@0D0AResearch/getting-started-with-velociraptor-part-2-b0d1aabe74e1)


## Conclusion

You have now successfully deployed Velociraptor EDR on your network. The dashboard allows you to manage and monitor all connected clients, and the server will ensure that your clients are securely connected and functioning as expected.# Velociraptor-Deployment
