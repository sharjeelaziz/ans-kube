# Kubernetes Cluster on Raspberry PIs with Ansible

Update `all.yml` with your desired Kubernetes package version. You can  get available package versions with the following command:

    curl -s https://packages.cloud.google.com/apt/dists/kubernetes-xenial/main/binary-amd64/Packages | grep Version | awk '{print $2}'


### Setup Cluster
1. Modify the inventory to suit your environment. Please refer to the setup/README.md for initial setup.
2. Verify that Ansible is running with ```ansible -m ping all```
3. Setup the cluster with ```ansible-playbook cluster.yml```
4. Configure the pi user to run kubectl  on master

        mkdir -p $HOME/.kube
        sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
        sudo chown $(id -u):$(id -g) $HOME/.kube/config


### Setup Dashboard
Screen is a console application that allows you to use multiple terminal sessions within one terminal window. We will run kubectl proxy in a screen session and use SSH port forwarding for tunneling the proxy port to our workstation.

1. Setup Kubernetes dashboard with ```ansible-playbook dashboard.yml```
2. Install Screen on the master node with ```sudo apt-get install screen```. Create a new screen session and run ```sudo kubectl proxy``` on the master node.
2. On your workstation open a terminal session with the command ```ssh -L 8001:127.0.01:8001 pi@<master node IP>```
3.  You should be able to connect to the dashboard from you workstation now by going to  http://localhost:8001/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy

### References:
[rak8s (pronounced rackets - /ˈrækɪts/)](https://rak8s.io/)
