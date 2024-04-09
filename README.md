# Deploying Application on Red Hat Linux EC2 Instance

This guide provides instructions on how to deploy an application on a Red Hat Linux server in AWS using EC2 user data. It also covers creating an Application Load Balancer (ALB) to distribute incoming application traffic across multiple instances.

## Prerequisites

- An AWS account
- Basic knowledge of EC2 and ALB

## Step 1: Launch an EC2 Instance with User Data

1. Open the AWS Management Console.
2. Navigate to the EC2 Dashboard and click on "Launch Instance".
3. Choose a Red Hat Enterprise Linux (RHEL) AMI.
4. Select an instance type.
5. In the "Configure Instance Details" step, expand the "Advanced Details" section.
6. Paste the application startup script into the "User data" text box. The script should include commands to install your application and any dependencies, start the application, etc.
```bash
   #!/bin/bash
# Update the system
yum update -y

# Install Apache (httpd)
yum install -y httpd

# Start the Apache service
systemctl start httpd.service

# Enable Apache to start on boot
systemctl enable httpd.service

# Get the host's IP address or FQDN
HOST_IP=$(hostname -f)

# Get the current date
CURRENT_DATE=$(date '+%Y-%m-%d %H:%M:%S')

# Generate the HTML content with styling
cat > /var/www/html/index.html <<EOF
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome to Hill-Top Consultancy DevOps CLASS Of 2024A</title>
    <style>
        body {
            background-color: #f0f0f0;
            text-align: center;
            font-family: Arial, sans-serif;
            font-size: 18px; /* Increase base font size */
        }
        h1, h2, p {
            margin: 20px 0; /* Adjust spacing */
        }
        h1 {
            color: #007bff;
            font-weight: bold;
            font-size: 32px; /* Increase font size for h1 */
        }
        h2 {
            color: #007bff;
            font-weight: bold;
            font-size: 24px; /* Increase font size for h2 */
        }
        .content {
            background-color: #ffffff;
            margin: auto;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            width: 80%;
            max-width: 600px;
        }
    </style>
</head>
<body>
    <div class="content">
        <h1>Welcome to Hill-Top Consultancy DevOps CLASS Of 2024A</h1>
        <h2>This is the Current Host IP Address: $HOST_IP</h2>
        <p>Current Date and Time: $CURRENT_DATE</p>
        <p>Hill-Top Consultancy is a premier IT training and consulting firm that was founded with the vision of empowering professionals and organizations
            by providing them with cutting-edge skills in DevOps, Cloud Computing, and Software Development. Our ethos is built on the foundation of continuous
            learning and innovation, which we believe are essential in navigating the ever-evolving technology landscape.</p>
        <p><strong>Email:</strong> info@htconsultancy.net</p>
        <p><strong>Phone:</strong> +45 715 740 47</p>
        <p><strong>Address:</strong> 2630 Taastrup, Denmark</p>
    </div>
</body>
</html>
EOF

# Restart Apache to apply changes
systemctl restart httpd.service
```

´´select the number of instances´´

7. Continue with the instance setup (add storage, configure security group to allow HTTP access on port 80, review, and launch).
8. Choose an existing key pair or create a new one, then launch your instance.

## Step 2: Create an Application Load Balancer

1. Navigate to the EC2 Dashboard and select "Load Balancers" under the "Load Balancing" section.
2. Click "Create Load Balancer" and select "Application Load Balancer".
3. Enter the load balancer name, choose internet-facing, and select the VPC and subnets.
4. Configure the security settings (if HTTPS, upload your SSL certificate).
5. Set up a security group that allows port 80 (HTTP) or 443 (HTTPS).
6. Configure routing to a new or existing target group. Specify the protocol (HTTP/HTTPS), port, and health check settings.
7. Register the EC2 instance(s) you launched with the user data script to the target group.
8. Review and create the load balancer.

## Step 3: Access the Application

- After the ALB is created, it will perform health checks on the registered instances. Once an instance passes the health check, it will start routing traffic to it.
- Find the DNS name of the ALB in the Load Balancer section of the EC2 Dashboard.
- Access the application via a web browser using the ALB DNS name. 
