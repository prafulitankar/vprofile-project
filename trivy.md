## INSTALLATION STEPS 

``` groovy
sudo apt-get install wget apt-transport-https gnupg lsb-release

wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null

echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list

sudo apt-get update

sudo apt-get install trivy
```
## COMMANDS---

``` bash
trivy image imagename    # To scan the Image
```

``` bash
trivy fs --security-checks vuln,config   Folder_name_OR_Path  #Scan the Local Git Repository
```

``` bash
trivy image --severity HIGH,CRITICAL image_name  #This will show only HIGH/CRITICAL Vulnerability
```

``` bash
trivy image -f json -o results.json image_name  # It will generate a Report in json format and store in result.json -f:file -o:output
```

``` bash
trivy repo repo-url #Scan the Remote Git Repository
```

``` bash
trivy k8s --report summary cluster #Scan the K8S Cluster
```
