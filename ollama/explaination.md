# Ollama with GPU in Docker

This guide provides step-by-step instructions to configure the NVIDIA Container Toolkit, set up Docker, and run a container using a YAML file.

---

## Prerequisites

- A supported container engine (Docker) must be installed.
- The NVIDIA Container Toolkit must be installed.

---

## Step 1: Configure the Production Repository

Run the following commands to configure the repository for the NVIDIA Container Toolkit:

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

### Enable Experimental Packages

To enable experimental packages, uncomment the experimental line in the repository configuration:

```bash
sudo sed -i -e '/experimental/ s/^#//g' /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

---

## Step 2: Update the Package List

Update the package list from the repository:

```bash
sudo apt-get update
```

---

## Step 3: Install the NVIDIA Container Toolkit

Install the NVIDIA Container Toolkit packages:

```bash
sudo apt-get install -y nvidia-container-toolkit
```

---

## Step 4: Configure Docker

Configure the container runtime using the `nvidia-ctk` command:

```bash
sudo nvidia-ctk runtime configure --runtime=docker
```

This command modifies the `/etc/docker/daemon.json` file to enable Docker to use the NVIDIA Container Runtime.

Restart the Docker daemon to apply the changes:

```bash
sudo systemctl restart docker
```

---

## Step 5: Create and Run the Container

Use the provided YAML file to create and start the container:

```bash
docker-compose -f ollama_gpu.yaml up -d
```

---

## Step 6: Access the Container

To access the container's bash shell, use the following command:

```bash
docker exec -it <container_id> bash
```

Replace `<container_id>` with the actual container ID.

---

## Step 7: Run the Model

Inside the container, run the desired model using the following command:

```bash
ollama run <model_name>
```

Replace `<model_name>` with the name of the model you want to run.

---

## Summary

This guide walks you through configuring the NVIDIA Container Toolkit, setting up Docker, and running a container with GPU support. Follow the steps to ensure proper setup and execution of your GPU-accelerated applications.

For more information, refer to the [NVIDIA Container Toolkit documentation](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/overview.html).