Python Deploy App Cloud (AWS EC2)
1. Create / launch your EC2 virtual machine

Ubuntu instance on AWS.

Make sure the security group allows:

SSH (22)
App port (5000 or 80 if using Nginx)
2. Connect from Mac (Terminal)
ssh -i ~/.ssh/your-key.pem ubuntu@YOUR_EC2_IP
3. Copy project from Mac to EC2 (secure copy)
scp -i ~/.ssh/your-key.pem ~/Downloads/Flask_Weather.zip ubuntu@YOUR_EC2_IP:/home/ubuntu/
4. SSH into the server
ssh -i ~/.ssh/your-key.pem ubuntu@YOUR_EC2_IP
5. Update server and install dependencies
sudo apt update -y
sudo apt install -y python3 python3-pip unzip python3-venv

Install YAML support:

pip install pyyaml
6. Create virtual environment
python3 -m venv venv
source venv/bin/activate
7. Verify zip file exists
ls
8. Unzip project
unzip Flask_Weather.zip
9. Enter project folder
cd Flask_Weather
10. Install Python dependencies
pip install flask requests pyyaml
11. Edit Flask file
nano flask_weather.py

Change:

app.run()

to:

app.run(host="0.0.0.0", port=5000, debug=True)
12. Run the app
python3 flask_weather.py
13. Kill old processes if port is in use
sudo fuser -k 5000/tcp
14. Access app in browser
http://YOUR_EC2_PUBLIC_IP:5000
