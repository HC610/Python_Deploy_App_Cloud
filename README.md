# Python Deploy App Cloud (AWS EC2)

## Overview

This guide explains how to deploy a Flask application on an AWS EC2 Ubuntu instance.

## Prerequisites

- AWS EC2 Ubuntu instance
- SSH key pair (`.pem` file)
- Flask project zipped (e.g. `Flask_Weather.zip`)
- Security Group allowing:
  - SSH (22)
  - TCP 5000 (or HTTP 80 if using Nginx)

## Deployment Steps

### 1. Connect to your EC2 instance

```bash
ssh -i ~/.ssh/your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

### 2. Copy your Flask project from your Mac to EC2

Run this from your **Mac terminal**:

```bash
scp -i ~/.ssh/your-key.pem ~/Downloads/Flask_Weather.zip ubuntu@YOUR_EC2_PUBLIC_IP:/home/ubuntu/
```

### 3. SSH back into your EC2 instance

```bash
ssh -i ~/.ssh/your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

### 4. Update Ubuntu and install required packages

```bash
sudo apt update -y
sudo apt install -y python3 python3-pip python3-venv unzip
```

### 5. Check that the zip file was copied successfully

```bash
ls
```

You should see:

```text
Flask_Weather.zip
```

### 6. Unzip the project

```bash
unzip Flask_Weather.zip
```

### 7. Enter the project folder

```bash
cd Flask_Weather
```

### 8. Create a Python virtual environment

```bash
python3 -m venv venv
```

### 9. Activate the virtual environment

```bash
source venv/bin/activate
```

You should now see:

```text
(venv)
```

### 10. Install the required Python packages

```bash
pip install flask requests pyyaml
```

Or, if your project contains a `requirements.txt` file:

```bash
pip install -r requirements.txt
```

### 11. Edit the Flask application

```bash
nano flask_weather.py
```

Find:

```python
app.run(debug=True)
```

Replace it with:

```python
app.run(host="0.0.0.0", port=5000, debug=True)
```

Save the file:

- `Ctrl + O`
- Press `Enter`
- `Ctrl + X`

### 12. If port 5000 is already in use, stop the existing process

```bash
sudo fuser -k 5000/tcp
```

### 13. Start the Flask application

```bash
python3 flask_weather.py
```

You should see output similar to:

```text
* Running on http://0.0.0.0:5000
```

### 14. Access the application

Open your browser and navigate to:

```text
http://YOUR_EC2_PUBLIC_IP:5000
```

## Architecture Diagram

Add your architecture image to an `images` folder in your repository and reference it like this:

```markdown
![Flask Deployment Architecture](images/flask-deployment.png)
```
