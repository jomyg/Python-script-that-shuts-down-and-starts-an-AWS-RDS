# Python-script-that-shuts-down-and-starts-an-AWS-RDS

### Python script that shuts down and starts an AWS RDS instance according to the schedule you specified, you can use the AWS SDK for Python (Boto3). Make sure you have Boto3 installed and configured with your AWS credentials. 

### You can install it using pip if you haven't already:

```
pip install boto3
```

### Here's a Python script to achieve

```
import boto3
import datetime
import time

# RDS instance information
rds_instance_id = 'rds-instance-id'
region_name = 'aws-region'

# Create an RDS client
client = boto3.client('rds', region_name=region_name)

# Define the schedule for shutdown and start
shutdown_time_weekday = datetime.time(19, 0)  # 7:00 PM IST
start_time_weekday = datetime.time(8, 0)     # 8:00 AM IST
shutdown_time_weekend = datetime.time(19, 0)  # 7:00 PM IST
start_time_monday = datetime.time(8, 0)      # 8:00 AM IST

def is_weekend():
    # Check if today is Friday (4) or Saturday (5) or Sunday (6)
    return datetime.datetime.today().weekday() in [4, 5, 6]

while True:
    current_time = datetime.datetime.now().time()
    
    # Check if it's a weekday or weekend
    if is_weekend():
        if current_time >= shutdown_time_weekend:
            # Shutdown the RDS instance
            print(f"Shutting down RDS instance {rds_instance_id}")
            client.stop_db_instance(DBInstanceIdentifier=rds_instance_id)
        elif current_time >= start_time_monday:
            # Start the RDS instance
            print(f"Starting RDS instance {rds_instance_id}")
            client.start_db_instance(DBInstanceIdentifier=rds_instance_id)
    else:
        if current_time >= shutdown_time_weekday:
            # Shutdown the RDS instance
            print(f"Shutting down RDS instance {rds_instance_id}")
            client.stop_db_instance(DBInstanceIdentifier=rds_instance_id)
        elif current_time >= start_time_weekday:
            # Start the RDS instance
            print(f"Starting RDS instance {rds_instance_id}")
            client.start_db_instance(DBInstanceIdentifier=rds_instance_id)
    
    # Sleep for a minute before checking the time again
    time.sleep(60)

```
