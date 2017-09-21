### Before You Begin

1.  Install Virtual Box [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
2.  Install Vagrant [https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)
3.  Clone this [git repo](https://github.com/jdavis7257/VagrantKubernetesBase) which contains the files necessary to stand up the cluster.

### Let's Go!

1.  From the root of the git repo cloned above. Run `vagrant up`
2.  Once the machines boot and the vagrant command returns you should now have two VMs running. SSH into the master by typing `vagrant ssh master`
3.  Once you get logged into the VM run `sudo kubeadm init --apiserver-advertise-address 10.0.3.15` if all goes well this will give you a functional kubernetes master listening on 10.0.3.15 which is the internal network between the master and node.
4.  You now need to setup the kubectl configs with this command `mkdir -p .kube && sudo cp -i /etc/kubernetes/admin.conf ~/.kube/config && sudo chown $(id -u):$(id -g) ~/.kube/config`
5.  After this returns you'll see a command that looks something like this `kubeadm join --token <tokenstring>  10.0.3.15:6443`. Copy this command from the terminal output as you will need it to join the node to the cluster.
6.  Type `exit` to leave the master ssh session for now.
7.  Type `vagrant ssh node`
8.  Switch to root `sudo -i`
9.  Paste the kubeadm join command you copied earlier and execute it. It should display Node join complete with some info about certificates and security. Now type `exit` twice.
10.  Now that you have a node we need to enable networking to get things started. Log into the master again with `vagrant ssh master`
11.  Run the following to setup the flannel network required utilize the cluster `kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml && kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel-rbac.yml` Note: You can also run several other pod networks by changing this step. I have not tested any of the others, but you can look at [this page](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-network) for more info on setting them up.
12.  Now run `kubectl get nodes`. You should see both the master and node listed. They may show NotReady at first, but should eventually report as Ready once the networking components have come online.

That's it! If all completed successfully your should now have a functional Kubernetes cluster. The default resource allocation is 2 vCPUs and 3GB of RAM. Feel free to adjust these in the vagrant file before creating. It's also worth noting that this should also work on Mac or Windows.