
## **2. Install the New Version**

## **For Linux:**

1. **Remove the Old Version**:
    
    `bashsudo rm /usr/local/bin/terraform`
    
2. **Download and Install**:You can use **`wget`** or **`curl`** to download the latest version directly to your terminal:
    
    `bashwget <https://releases.hashicorp.com/terraform/1.10.5/terraform_1.10.5_linux_amd64.zip`>
    
3. **Unzip the Downloaded File**:
    
    `bashunzip terraform_1.10.5_linux_amd64.zip`
    
4. **Move Terraform to a Directory in Your PATH**:
    
    `bashsudo mv terraform /usr/local/bin/`
    
5. **Verify the Installation**:After installation, verify that Terraform has been updated successfully:
    
    `bashterraform version`