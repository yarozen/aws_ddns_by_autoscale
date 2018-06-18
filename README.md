### This AWS Lambda function gets triggered on every auto scaling group CloudWatch Event being watched and creates/updates Route 53 Private Hosted Zone with A record set of all the Private IPs of the running instances in that auto scaling group.

##### In details:
- An Auto Scaling event triggered the Lambda function (Instance Launched / Instance Terminated)
- Checks if the Hosted Zone `example.internal.` exists for same VPC from which the auto scaling group event was triggered
- If Hosted Zone exists it obtains its ID
- Else (Hosted Zone doesn't exist) it creates the `example.internal.` Hosted Zone and obtains its ID
- Next it obtains a list of private IPs of all active instances in the auto scaling group which triggered this event
- If the list contains at least one instance it creates or updates a record set named `<autoscaling_group_name>.example.internal.` with the private IPs of the instances as A records
- Else the list is empty (which means the auto scaling group was deleted or doesn't contain any active instance) it deletes the record set named `<autoscaling_group_name>.example.internal.`
- `example.internal.` Hosted Zone can be renamed as desired in the py script
    
##### Files:
- `aws_autoscaling_cloudwatch_event.json` - Example of CloudWatch Events to trigger the Lambda Function
- `aws_autoscaling_execution_role.json` - The permissions (Execution role) required for the lambda function to operate
- `aws_autoscaling_lambda_function.py` - the actual Lambda function (written in Python 2.7)
