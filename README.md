Description (TL;DR):
This AWS lambda function gets triggered on every autoscaling group event being watched and creates or updates an A record set of all the Private IPs of the running instances in that autoscaling group.

In details:
    - An Auto Scaling event has occured and triggered the Lambda function (Instance Launched / Instance Terminated)
    - It checks if the Hosted Zone 'example.internal.' exists for same VPC from which the autosclaing group event was triggered
    - If Hosted Zone exists it obtains its ID
    - Else (Hosted Zone doesn't exist) it creates the 'example.internal.' Hosted Zone and obtains its ID
    - Next it obtains a list of private IPs of all active instances in the autoscaling group which triggered this event
    - If the list contains at least one instance it creates or updates a record set named '<autoscaling_group_name>.example.internal.' with the private IPs of the instances as A records
    - Else the list is empty (which means the auto scaling group was deleted or doesnt contain any active instance) it deletes the record set named '<autoscaling_group_name>.example.internal.'
    * 'example.internal.' Hosted Zone can be renamed
    
Files:
    - aws_autoscaling_cloudwatch_event.json - Example of CloudWatch Events to trigger the Lambda Function
    - aws_autoscaling_execution_role.json - The permissions (Execution role) required for the lambda function to operate
    - aws_autoscaling_lambda_function.py - the actual Lambda function (written in Python 2.7)
