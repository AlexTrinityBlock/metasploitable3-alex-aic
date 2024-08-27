# 介紹

本腳本是用於資訊安全教育用途，在 Ubuntu 24.04 上安裝 metasploitable3 的過程。

# 1. 用 Gcloud 建立允許虛擬化的 Image

```gcloud
gcloud compute images create ubuntu-nested-vm-image --source-image=ubuntu-2404-noble-amd64-v20240821 --source-image-project=ubuntu-os-cloud  --licenses="https://www.googleapis.com/compute/v1/projects/vm-options/global/licenses/enable-vmx"
```

# 2. 建立 VM 用上述的 Image

選擇 VM 類型 n2-standard-4，使用剛剛的 Image: ubuntu-nested-vm-image，並且設置約莫 65 GB

# 3. 安裝 virtualbox 與 vagrant

安裝 virtualbox

```bash
sudo apt update && \
sudo apt install virtualbox -y
```

安裝 vagrant

```bash
sudo apt install -y gnupg software-properties-common && \
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg  &&\  
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list && \
sudo apt update  && \
sudo apt install vagrant -y
```

建立 VM 內部網段

```bash
sudo mkdir /etc/vbox/ && \
sudo touch /etc/vbox/networks.conf && \
echo "* 10.0.0.0/8 192.168.0.0/16 172.16.0.0/12" | sudo tee -a /etc/vbox/networks.conf
```

下載並且啟用 Vagrant

```bash
mkdir metasploitable3-workspace && \
cd metasploitable3-workspace && \
curl -O https://raw.githubusercontent.com/AlexTrinityBlock/metasploitable3-alex-aic/master/Vagrantfile && vagrant up
```

或者修改為，僅開啟 Ubuntu 靶機

```bash
curl -O https://raw.githubusercontent.com/AlexTrinityBlock/metasploitable3-alex-aic/master/Vagrantfile && vagrant up ub1404
```
