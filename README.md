# Terraform Task

### Terraform script to create AWS s3 bucket and 2 files in it with the timestamp when the script was executed.
### Terratest to check the above actions are executed correctly.
### 1. Manual

### Prerequisites
* [Terraform](https://www.terraform.io/downloads.html)
* [Go](https://golang.org/dl/)
* Export AWS access keys to Github to be used in github actions.

1. On GitHub, navigate to the main page of the repository.
2. Under your repository name, click  Settings.
3. Repository settings button
4. In the left sidebar, click Secrets.
5. Click New repository secret.
6. Type a name for your secret in the Name input box.
7. Enter the value for your secret.
8. Click Add secret.


### Steps
* Initializing terraform modules.
 ```sh
  terraform init
  ``` 

* Check what changes will occur once ran.
 ```sh
  terraform plan
  ``` 
* Apply changes.
 ```sh
  terraform apply -auto-approve
  ``` 

* You should find a new S3 bucket created with 2 files with the timestamp in them.


### Use Terratest to apply the changes and test if the bucket and the files exist or not.
* It will run both terraform init and terraform apply to create a bucket with a random name then run tests to check if the bucket and the files were created successfully, it will destroy everything after it compeletes and it should return "PASS" at the end.
 ```sh
  cd test
  go get "needed dependencies"
  go test -v
  ```
  
### 2. Github actions (automated).
  
 * After adding your AWS keys to github secrets, workflow can use them to create the S3 bucket and list whats inside of it.
 
  ```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    env:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

* Workflow is configured to run once a push is made to task branch.
```yaml
on: 
  push:
    branches:
      - task
```


* It will clone the repo, install Go, install the dependencies and test it for you.

```yaml
    - name: Test
      working-directory: /home/runner/work/terraformTask/terraformTask/test
      run: go test -v
```

