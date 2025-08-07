# Create Nginx Docker Container Using Terraform

This project demonstrates how to use Terraform to create a Docker container running the latest Nginx image on port `8081`.

---

## 🧰 Tools Used

- **Terraform**
- **Docker**
- **Ubuntu (Local Machine)**

---

## 📦 Step-by-Step Process

### Step 1: Create Project Folder & Files

```bash
mkdir task-3-terraform-docker
cd task-3-terraform-docker
touch main.tf
```
<img width="700" height="136" alt="Screenshot from 2025-08-07 13-18-08" src="https://github.com/user-attachments/assets/4f0e3b53-4ffa-4e58-9948-1be7d22e00b2" />

- Then open it using any text editor (like nano, code, or VS Code), and paste this code inside main.tf:
```bash
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 2.23.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name = "nginx:latest"
}

resource "docker_container" "nginx" {
  name  = "nginx_container"
  image = docker_image.nginx.latest
  ports {
    internal = 80
    external = 8081
  }
}
```
<img width="491" height="343" alt="Screenshot from 2025-08-07 13-18-33" src="https://github.com/user-attachments/assets/02db3dac-fbce-49ff-8ba2-80d73d9d4f3f" />

---

### Step 2: Install Terraform
```bash
sudo snap install terraform --classic
```
<img width="857" height="100" alt="Screenshot from 2025-08-07 14-31-39" src="https://github.com/user-attachments/assets/1294fa10-8aac-4d01-8516-3653e2464427" />

---

### Step 3: Initialize Terraform
- In the same directory where main.tf is saved, run: ```terraform init``` 
<br><br>
<img width="701" height="438" alt="Screenshot from 2025-08-07 13-19-33" src="https://github.com/user-attachments/assets/1e44a1ef-d95a-49ab-a30c-5f9810345aec" />

---

### Step 4: Apply the Configuration (Start Container)
- Now let’s actually provision the infrastructure.
Run: ``terraform plan``
<img width="1000" height="225" alt="Screenshot from 2025-08-07 13-21-04" src="https://github.com/user-attachments/assets/c8bd050c-1486-46eb-b5ca-61385fafaaf5" />
<img width="1000" height="375" alt="Screenshot from 2025-08-07 13-21-23" src="https://github.com/user-attachments/assets/c9ee170b-81a1-490c-9fe4-de075548d7b4" />

---

### Step 5: Apply the Configuration (Start Container)
- Now let’s actually provision the infrastructure.
Run: ``terraform apply``
<img width="1000" height="203" alt="Screenshot from 2025-08-07 13-22-05" src="https://github.com/user-attachments/assets/983dc718-6bcf-44d9-904e-4d8525d0bee3" />

<br>

- It will ask:
```vbnet
Do you want to perform these actions?
  Terraform will perform the following actions:
    ...
  Enter a value: 
```
- Type: ``yes``
<img width="513" height="100" alt="Screenshot from 2025-08-07 13-22-27" src="https://github.com/user-attachments/assets/6409e91c-6e71-4f02-a236-77466958b178" />
<img width="1000" height="510" alt="Screenshot from 2025-08-07 13-23-41" src="https://github.com/user-attachments/assets/eddb2e88-62f2-4004-8276-b1986c845806" />

<br>

---

### Step 6: Check container is running: 
``docker ps``

<br>

<img width="1000" height="101" alt="Screenshot from 2025-08-07 13-27-33" src="https://github.com/user-attachments/assets/86f89f90-182e-4576-b8e3-388a41dbbc82" />

<br>

### Open in browser:
- Visit: ``http://localhost:8081``
- You should see the default Nginx welcome page.

<br>

<img width="1000" height="212" alt="Screenshot from 2025-08-07 13-27-47" src="https://github.com/user-attachments/assets/2f95c25e-7560-44d2-ad09-8f41a66c75a1" />

<br>

---

### Step 7: Explore Terraform State
- Terraform keeps track of all created resources using a file called *terraform.tfstate*.
- View the state file
- In terminal: ``ls``
- You will see: ``main.tf`` & ``terraform.tfstate``  
- List resources managed by Terraform
```bash
terraform state list
```
- Output will show:
```bash
docker_image.nginx
docker_container.nginx
```
<img width="785" height="159" alt="Screenshot from 2025-08-07 13-28-44" src="https://github.com/user-attachments/assets/c5428dea-b3d0-4443-b328-d77c9681f191" />

<br>

- Remove docker container by typing command:
```bash
sudo docker rm -f nginx_container
```
<img width="708" height="50" alt="Screenshot from 2025-08-07 13-44-58" src="https://github.com/user-attachments/assets/8fad6fb3-adfc-4ce9-884e-bb3c224a4643" />

---

### Step 8: Destroy the Infrastructure (Cleanup)
- Terraform gives you full control — so let’s clean everything it created.
- In your terminal, run:
```bash
terraform destroy
```
<img width="1000" height="249" alt="Screenshot from 2025-08-07 13-30-09" src="https://github.com/user-attachments/assets/980258be-e490-47f2-a94d-d070ca69d56b" />

- It will show what resources will be destroyed:
```yaml
Terraform will destroy the following:
  # docker_container.nginx will be destroyed
  # docker_image.nginx will be destroyed
```
- Then it will ask:
```pgsql
Do you really want to destroy all resources? (yes/no)
```
- Type : ``yes``
<img width="1000" height="245" alt="Screenshot from 2025-08-07 13-45-12" src="https://github.com/user-attachments/assets/8d07a87e-de60-4d46-ba38-b67461616610" />
<img width="1000" height="254" alt="Screenshot from 2025-08-07 13-45-25" src="https://github.com/user-attachments/assets/7140b1f3-6480-4573-ad5c-10a797f1e117" />

