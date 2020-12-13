import boto3
import datetime
from dateutil.parser import parse

def age_days(date):
	ami_date = parse(date).replace(tzinfo=None)
	diff = datetime.datetime.now() - ami_date
	return diff.days

def lambda_handler(event, context):
    def send_sns():
        sns = boto3.resource('sns')
        topic = sns.Topic('arn:aws:sns:us-east-1:354064172755:Billingalarm')
        response = topic.publish(Message='ImageId: {}, with name: {} and CreationDate: {} exist in your account. \nKindly review.'.format(image_id, image_name, creation_date))
        return response

    ec2_client = boto3.client('ec2', region_name='us-east-1') # Change region appropriate for current use case.
    images = ec2_client.describe_images(Owners=['self'])['Images']
    for ami in images:
        #Image variables
        creation_date = ami['CreationDate']
        image_id = ami['ImageId']
        image_name = ami['Name']
        if age_days(creation_date) < 2:
            send_sns()
        else:
            pass
