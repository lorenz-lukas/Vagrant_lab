# Vagrant_lab
Vagrant Lab with Ansible, K8s and Docker. Docker image with Vuejs example.

# Install packages

    - https://www.virtualbox.org/wiki/Linux_Downloads

    - https://helm.sh/docs/intro/install/

    - https://minikube.sigs.k8s.io/docs/start/

    - https://kind.sigs.k8s.io/docs/user/quick-start/

    - https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

    - https://www.vagrantup.com/downloads

    - pip3 install ansible

    - https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-pt

# Starting a VM with Vagrant

    - Ubuntu 20: sudo apt-get install libarchive-tools

    - vagrant up

# Generate a ssh-key and copy to VM

    - ssh-keygen -t rsa -C "Vagrant Lab"

    - ls ~/.ssh/

    - cat ~/.ssh/vagrant_id
    
    - cat ~/.ssh/vagrant_id.pub

    - ssh -i ./.vagrant/machines/default/virtualbox/private_key vagrant@192.168.33.10

    - scp -i ./.vagrant/machines/default/virtualbox/private_key -r ~/.ssh/vagrant_id.pub vagrant@192.168.33.10:/home/vagrant/.ssh/authorized_keys

    - ssh -i ~/.ssh/vagrant_id vagrant@192.168.33.10

# Running

    - ansible-playbook master.yml

    - cd docker && sudo docker build . --label vagrant --tag vagrant_lab && cd ..

    - sudo docker push lorenzlukas/docker_image_examples:0.1.0-vagrant

    - sudo docker run -rm -p 5000:5000 -p 8888:8888 --name=vagrant vagrant_lab:latest

    - sudo docker rmi $(sudo docker image ls | grep none | head -n-1 | awk '{print $3}')

# Creating helm to deploy

    - mkdir helm && cd helm && helm create lab-vagrant && cd ..

    - cp kubernetes/{configmap.yaml, deploy.yaml, secrets.yaml, services.yaml} helm/templates/

    (https://helm.sh/docs/topics/charts/)

    (https://www.youtube.com/watch?v=5_J7RWLLVeQ)

# Check helm

    - cd helm && helm template lab-vagrant lab-vagrant && cd ..

# Deploy helm

    - cd helm && helm install lab-vagrant lab-vagrant && helm list && cd ..
    
    - cd helm && helm upgrade lab-vagrant lab-vagrant --values ./lab-vagrant/values.yaml && cd ..

# Yaml hints

    - set values.yaml:

        deployment:
            image: "lorenzlukas/docker_image_examples:0.1.0-vagrant"
            tag: "1.0.1"
            name: lab-vagrant
    
    - Then, in any file it is possible to access this infos using:
        
        EX: deployment.yaml
        
        containers:
            - name: "{{ .Values.deployment.name }}"
            image: {{ .Values.deployment.image }}:{{ .Values.deployment.tag }}