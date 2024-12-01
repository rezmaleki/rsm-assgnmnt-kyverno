# Kyverno Policy Testing

This Directory contains Kyverno policiy and test resources to validate  configurations. Follow the guide below to use and test the files in this repository.

---

### **1. Prerequisites**
- **Kyverno CLI**
  Install the Kyverno CLI from the [official documentation](https://kyverno.io/docs/installation/#install-the-cli):

  ```bash
  curl -L https://github.com/kyverno/kyverno/releases/latest/download/kyverno-cli-linux.tar.gz -o kyverno-cli.tar.gz
  tar -xvzf kyverno-cli.tar.gz -C /usr/local/bin```

- **GIT**  
Install Git to clone the repository or reference the raw files directly.

### 2. Running Tests Directly from GitHub 

You can run Kyverno tests directly using the raw URL of the test file:
```bash
kyverno test kyverno test https://github.com/rezmaleki/rsm-assgnmnt-kyverno.git
```
### 3. Running Tests Locally

To run tests locally using a cloned copy of the repository:
	1.	**Clone the repository**:
 
 Clone this repository using the command below:
 ```bash
 git clone https://github.com/rezmaleki/rsm-assgnmnt-kyverno.git
 cd rsm-assgnmnt-kyverno  
```

2.**Execute tests:**

Use the following command to run Kyverno tests locally:


```bash
kyverno test tests/kyverno-test.yaml
```


<img width="1078" alt="image" src="https://github.com/user-attachments/assets/23625b36-55d6-484f-bb93-943885203b4f">
